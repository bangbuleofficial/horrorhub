{
  "name": "horror-hub-indonesia",
  "version": "1.0.0",
  "scripts": {
    "dev": "NODE_ENV=development tsx server/index.ts",
    "build": "vite build && tsc server/index.ts --outDir dist",
    "start": "node dist/index.js",
    "db:push": "drizzle-kit push"
  }
}
Database Schema (shared/schema.ts)
import { sql } from "drizzle-orm";
import { pgTable, text, varchar, timestamp, integer, json } from "drizzle-orm/pg-core";
import { createInsertSchema } from "drizzle-zod";
import { z } from "zod";
export const horrorPosts = pgTable("horror_posts", {
  id: varchar("id").primaryKey().default(sql`gen_random_uuid()`),
  platform: varchar("platform", { length: 50 }).notNull(),
  author: text("author").notNull(),
  content: text("content").notNull(),
  imageUrl: text("image_url"),
  videoUrl: text("video_url"),
  metrics: json("metrics").$type<{
    likes: number;
    comments: number;
    shares: number;
    views?: number;
  }>().notNull(),
  isViral: integer("is_viral").default(0),
  createdAt: timestamp("created_at").defaultNow().notNull(),
});
export const insertHorrorPostSchema = createInsertSchema(horrorPosts).omit({
  id: true,
  createdAt: true,
});
export type InsertHorrorPost = z.infer<typeof insertHorrorPostSchema>;
export type HorrorPost = typeof horrorPosts.$inferSelect;
export const users = pgTable("users", {
  id: varchar("id").primaryKey().default(sql`gen_random_uuid()`),
  username: text("username").notNull().unique(),
  password: text("password").notNull(),
});
export const insertUserSchema = createInsertSchema(users).pick({
  username: true,
  password: true,
});
export type InsertUser = z.infer<typeof insertUserSchema>;
export type User = typeof users.$inferSelect;
Database Connection (server/db.ts)
import { Pool, neonConfig } from '@neondatabase/serverless';
import { drizzle } from 'drizzle-orm/neon-serverless';
import ws from "ws";
import * as schema from "@shared/schema";
neonConfig.webSocketConstructor = ws;
if (!process.env.DATABASE_URL) {
  throw new Error(
    "DATABASE_URL must be set. Did you forget to provision a database?",
  );
}
export const pool = new Pool({ connectionString: process.env.DATABASE_URL });
export const db = drizzle({ client: pool, schema });
Storage Layer (server/storage.ts)
import { users, type User, type InsertUser, horrorPosts, type HorrorPost, type InsertHorrorPost } from "@shared/schema";
import { db } from "./db";
import { eq, desc, ilike } from "drizzle-orm";
export interface IStorage {
  getUser(id: string): Promise<User | undefined>;
  getUserByUsername(username: string): Promise<User | undefined>;
  createUser(insertUser: InsertUser): Promise<User>;
  getHorrorPosts(platform?: string, limit?: number): Promise<HorrorPost[]>;
  createHorrorPost(insertPost: InsertHorrorPost): Promise<HorrorPost>;
  searchHorrorPosts(query: string): Promise<HorrorPost[]>;
}
export class DatabaseStorage implements IStorage {
  async getUser(id: string): Promise<User | undefined> {
    const [user] = await db.select().from(users).where(eq(users.id, id));
    return user || undefined;
  }
  async getUserByUsername(username: string): Promise<User | undefined> {
    const [user] = await db.select().from(users).where(eq(users.username, username));
    return user || undefined;
  }
  async createUser(insertUser: InsertUser): Promise<User> {
    const [user] = await db
      .insert(users)
      .values(insertUser)
      .returning();
    return user;
  }
  async getHorrorPosts(platform?: string, limit = 20): Promise<HorrorPost[]> {
    let baseQuery = db.select().from(horrorPosts);
    
    if (platform && platform !== 'all') {
      const posts = await baseQuery
        .where(eq(horrorPosts.platform, platform))
        .orderBy(desc(horrorPosts.createdAt))
        .limit(limit);
      return posts;
    }
    
    const posts = await baseQuery
      .orderBy(desc(horrorPosts.createdAt))
      .limit(limit);
    return posts;
  }
  async createHorrorPost(insertPost: InsertHorrorPost): Promise<HorrorPost> {
    const [post] = await db
      .insert(horrorPosts)
      .values({
        platform: insertPost.platform,
        author: insertPost.author,
        content: insertPost.content,
        imageUrl: insertPost.imageUrl ?? null,
        videoUrl: insertPost.videoUrl ?? null,
        metrics: {
          likes: insertPost.metrics.likes,
          comments: insertPost.metrics.comments,
          shares: insertPost.metrics.shares,
          views: insertPost.metrics.views as number | undefined,
        },
        isViral: insertPost.isViral ?? 0,
      })
      .returning();
    return post;
  }
  async searchHorrorPosts(query: string): Promise<HorrorPost[]> {
    const searchTerm = `%${query.toLowerCase()}%`;
    const posts = await db
      .select()
      .from(horrorPosts)
      .where(
        ilike(horrorPosts.content, searchTerm)
      )
      .orderBy(desc(horrorPosts.createdAt));
    
    return posts;
  }
  async initializeWithMockData(): Promise<void> {
    const existingPosts = await db.select().from(horrorPosts).limit(1);
    if (existingPosts.length > 0) {
      return;
    }
    const mockPosts: InsertHorrorPost[] = [
      {
        platform: "twitter",
        author: "@HorrorStoryID",
        content: "Thread: Kisah seram dari rumah adat Jawa ini bikin merinding. Katanya setiap malam Jumat Kliwon, terdengar suara gamelan dari dalam... 🧵👻 #HorrorIndonesia",
        imageUrl: "https://pixabay.com/get/ga1e30bdba6160e8d25a678e3ff1d5c8081211a0c1c650d3058d7fc8899350a6cd60293a757b9dcf419666c55392ab9a83702f2ed8682b8784f05f1013b9d5d4a_1280.jpg",
        metrics: { likes: 2100, comments: 456, shares: 892 },
        isViral: 1,
      },
      {
        platform: "instagram",
        author: "misterihoror_id",
        content: "Hutan Alas Roban, Jawa Tengah. Konon katanya setiap malam ada penampakan sosok putih berjalan di antara pohon-pohon... Siapa yang berani kesini? 👻🌲 #MisteriIndonesia #Horror",
        imageUrl: "https://pixabay.com/get/gfde1425fbf78229fa352a2dabbbbd0657c95ca011a905b0b25cb05c07cbd7d8b5e3edf285c3494cde025af17564b8d5c56c6a00bdabc96eee5d5daa2400fe472_1280.jpg",
        metrics: { likes: 3700, comments: 234, shares: 156 },
        isViral: 0,
      },
      {
        platform: "tiktok",
        author: "@ghosthunter_jkt",
        content: "Suara aneh dari makam tua ini... 😰",
        imageUrl: "https://images.unsplash.com/photo-1578662996442-48f60103fc96",
        videoUrl: "video-placeholder",
        metrics: { likes: 12300, comments: 1200, shares: 890, views: 45600 },
        isViral: 1,
      },
      {
        platform: "youtube",
        author: "Paranormal Indonesia",
        content: "EKSPLORASI GEDUNG TERBENGKALAI JAKARTA - SUARA ANEH DI LANTAI 13! 😱",
        imageUrl: "https://pixabay.com/get/g24cf95bb1fe4554f503d7c2583df0de65442587672c055936be4ef250f057b24050fc09fd98aec22d529fa48f30380e0a7266bacf8de3a58f528126ca5307181_1280.jpg",
        videoUrl: "video-placeholder",
        metrics: { likes: 3100, comments: 567, shares: 234, views: 45200 },
        isViral: 1,
      },
      {
        platform: "facebook",
        author: "Cerita Horor Nusantara",
        content: "PENGALAMAN PRIBADI: Malam itu saya sedang pulang dari kantor melalui Jalan Casablanca, Jakarta. Tiba-tiba motor saya mogok di depan kompleks yang katanya angker...",
        imageUrl: "https://images.unsplash.com/photo-1514525253161-7a46d19cd819",
        metrics: { likes: 1800, comments: 342, shares: 156 },
        isViral: 0,
      }
    ];
    for (const post of mockPosts) {
      await this.createHorrorPost(post);
    }
  }
}
export const storage = new DatabaseStorage();
API Routes (server/routes.ts)
import type { Express } from "express";
import { createServer, type Server } from "http";
import { storage } from "./storage";
export async function registerRoutes(app: Express): Promise<Server> {
  
  app.get("/api/horror-posts", async (req, res) => {
    try {
      const platform = req.query.platform as string;
      const limit = parseInt(req.query.limit as string) || 20;
      
      const posts = await storage.getHorrorPosts(platform, limit);
      res.json(posts);
    } catch (error) {
      res.status(500).json({ message: "Failed to fetch horror posts" });
    }
  });
  app.get("/api/horror-posts/search", async (req, res) => {
    try {
      const query = req.query.q as string;
      
      if (!query) {
        return res.status(400).json({ message: "Search query is required" });
      }
      const posts = await storage.searchHorrorPosts(query);
      res.json(posts);
    } catch (error) {
      res.status(500).json({ message: "Failed to search horror posts" });
    }
  });
  app.get("/api/horror-posts/fresh", async (req, res) => {
    try {
      const since = req.query.since as string;
      const sinceDate = since ? new Date(since) : new Date(Date.now() - 2 * 60 * 1000);
      
      const allPosts = await storage.getHorrorPosts();
      const freshPosts = allPosts.filter(post => post.createdAt > sinceDate);
      
      res.json(freshPosts);
    } catch (error) {
      res.status(500).json({ message: "Failed to fetch fresh content" });
    }
  });
  const httpServer = createServer(app);
  return httpServer;
}
Server Entry Point (server/index.ts)
import express, { type Request, Response, NextFunction } from "express";
import { registerRoutes } from "./routes";
import { setupVite, serveStatic, log } from "./vite";
import { storage } from "./storage";
const app = express();
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
(async () => {
  try {
    await (storage as any).initializeWithMockData();
    log("Database initialized with mock horror content");
  } catch (error) {
    log("Database initialization skipped or failed:", error);
  }
  const server = await registerRoutes(app);
  if (app.get("env") === "development") {
    await setupVite(app, server);
  } else {
    serveStatic(app);
  }
  const port = parseInt(process.env.PORT || '5000', 10);
  server.listen({
    port,
    host: "0.0.0.0",
    reusePort: true,
  }, () => {
    log(`serving on port ${port}`);
  });
})();
Frontend Home Page (client/src/pages/home.tsx)
import { useState, useEffect } from "react";
import { useQuery } from "@tanstack/react-query";
import { HorrorPost } from "@shared/schema";
import HeaderNav from "@/components/header-nav";
import FloatingPlatforms from "@/components/floating-platforms";
import FilterTabs from "@/components/filter-tabs";
import HorrorContentCard from "@/components/horror-content-card";
import { useAudioNotification } from "@/hooks/use-audio-notification";
import { Skeleton } from "@/components/ui/skeleton";
export default function Home() {
  const [selectedPlatform, setSelectedPlatform] = useState<string>("all");
  const [sortOrder, setSortOrder] = useState<string>("recent");
  const [searchQuery, setSearchQuery] = useState<string>("");
  const [viewMode, setViewMode] = useState<"grid" | "list">("grid");
  const [lastRefreshTime, setLastRefreshTime] = useState<Date>(new Date());
  const [showRefreshNotification, setShowRefreshNotification] = useState(false);
  const { playNotification } = useAudioNotification();
  const { data: posts, isLoading, refetch } = useQuery<HorrorPost[]>({
    queryKey: ["/api/horror-posts", selectedPlatform !== "all" ? `?platform=${selectedPlatform}` : ""],
    refetchInterval: 120000, // 2 minutes
  });
  const { data: searchResults } = useQuery<HorrorPost[]>({
    queryKey: ["/api/horror-posts/search", `?q=${searchQuery}`],
    enabled: searchQuery.length > 0,
  });
  const displayPosts = searchQuery.length > 0 ? searchResults : posts;
  useEffect(() => {
    const interval = setInterval(() => {
      refetch();
      setLastRefreshTime(new Date());
      setShowRefreshNotification(true);
      playNotification();
      
      setTimeout(() => {
        setShowRefreshNotification(false);
      }, 3000);
    }, 120000); // 2 minutes
    return () => clearInterval(interval);
  }, [refetch, playNotification]);
  const sortedPosts = displayPosts?.slice().sort((a, b) => {
    switch (sortOrder) {
      case "viral":
        return ((b.isViral ?? 0) - (a.isViral ?? 0)) || (b.metrics.likes - a.metrics.likes);
      case "engagement":
        const aEngagement = a.metrics.likes + a.metrics.comments + a.metrics.shares;
        const bEngagement = b.metrics.likes + b.metrics.comments + b.metrics.shares;
        return bEngagement - aEngagement;
      default: // recent
        return new Date(b.createdAt).getTime() - new Date(a.createdAt).getTime();
    }
  });
  return (
    <div className="min-h-screen bg-netflix text-white">
      <HeaderNav 
        searchQuery={searchQuery}
        onSearchChange={setSearchQuery}
        lastUpdateTime={`${Math.floor((Date.now() - lastRefreshTime.getTime()) / 60000)}m ago`}
      />
      
      <FloatingPlatforms 
        selectedPlatform={selectedPlatform}
        onPlatformSelect={setSelectedPlatform}
      />
      
      <FilterTabs
        selectedPlatform={selectedPlatform}
        onPlatformSelect={setSelectedPlatform}
        sortOrder={sortOrder}
        onSortChange={setSortOrder}
        viewMode={viewMode}
        onViewModeChange={setViewMode}
      />
      {showRefreshNotification && (
        <div className="fixed top-20 left-1/2 transform -translate-x-1/2 bg-green-600 text-white px-6 py-3 rounded-lg shadow-lg z-50">
          Konten baru telah dimuat!
        </div>
      )}
      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
        {isLoading ? (
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
            {Array.from({ length: 8 }).map((_, i) => (
              <div key={i} className="bg-gray-850 rounded-xl p-4">
                <Skeleton className="h-48 w-full mb-4 bg-gray-700" />
                <Skeleton className="h-4 w-3/4 mb-2 bg-gray-700" />
                <Skeleton className="h-4 w-1/2 bg-gray-700" />
              </div>
            ))}
          </div>
        ) : (
          <div className={viewMode === "grid" 
            ? "grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6" 
            : "space-y-4"
          }>
            {sortedPosts?.map((post) => (
              <HorrorContentCard 
                key={post.id} 
                post={post} 
                viewMode={viewMode}
              />
            ))}
          </div>
        )}
      </main>
    </div>
  );
}
CSS Styling (client/src/index.css)
@tailwind base;
@tailwind components;
@tailwind utilities;
:root {
  --netflix-bg: hsl(0, 0%, 7.8%);
  --horror-red: hsl(0, 73%, 41%);
  --gray-850: hsl(215, 20%, 12%);
}
@layer base {
  body {
    @apply font-sans antialiased bg-netflix text-white overflow-x-hidden;
    background-color: var(--netflix-bg);
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "Segoe UI", Roboto, sans-serif;
  }
}
@layer utilities {
  .bg-netflix {
    background-color: var(--netflix-bg);
  }
  
  .bg-horror {
    background-color: var(--horror-red);
  }
  
  .text-horror {
    color: var(--horror-red);
  }
  
  .bg-gray-850 {
    background-color: var(--gray-850);
  }
  
  .hover-scale:hover {
    transform: scale(1.05);
  }
  
  .content-card:hover {
    transform: translateY(-4px);
  }
}
Audio Notification Hook (client/src/hooks/use-audio-notification.tsx)
import { useCallback } from "react";
export function useAudioNotification() {
  const playNotification = useCallback(() => {
    try {
      const audioContext = new (window.AudioContext || (window as any).webkitAudioContext)();
      const oscillator = audioContext.createOscillator();
      const gainNode = audioContext.createGain();
      
      oscillator.connect(gainNode);
      gainNode.connect(audioContext.destination);
      
      oscillator.frequency.value = 800;
      oscillator.type = 'sine';
      
      gainNode.gain.setValueAtTime(0, audioContext.currentTime);
      gainNode.gain.linearRampToValueAtTime(0.1, audioContext.currentTime + 0.01);
      gainNode.gain.exponentialRampToValueAtTime(0.001, audioContext.currentTime + 0.3);
      
      oscillator.start(audioContext.currentTime);
      oscillator.stop(audioContext.currentTime + 0.3);
      
      console.log('🔔 Ding! New content notification played');
    } catch (error) {
      console.log('🔔 Audio notification (silent mode)');
    }
  }, []);
  return { playNotification };
}
