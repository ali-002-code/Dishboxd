# Dishboxd – AI-Powered Social Food Rating App

Live on the App Store: [Download Dishboxd](your-app-store-link)

## Overview
A full-stack iOS social app where users rate individual dishes rather than restaurants. Built from scratch and shipped to the App Store, featuring a computer vision AI pipeline, real-time social features, and a serverless backend architecture.

## Technical Architecture

### AI Pipeline
- **Food validation** — every uploaded image is sent to Claude Vision API via a Supabase Edge Function. The model classifies whether the image contains food/drink before allowing the post, rejecting non-food content in real time
- **Auto-categorisation** — a second vision model analyses the image alongside dish name and restaurant to classify into 18 cuisine categories (Italian, Japanese, Middle Eastern etc)
- Images are encoded as base64 in React Native, routed through serverless Edge Functions to keep API keys server-side

### Backend & Data
- PostgreSQL database with Row Level Security policies enforcing data access at the database level
- Real-time push notifications via a pg_net trigger pipeline: database INSERT → PostgreSQL trigger → net.http_post → Supabase Edge Function → Expo Push API → APNs
- Serverless Edge Functions (Deno/TypeScript) handling AI calls, push notifications, and account deletion

### Mobile
- React Native (Expo SDK 52) with TypeScript
- expo-router v4 for file-based navigation
- Custom image upload pipeline using base64 encoding + ArrayBuffer to handle React Native file URI limitations

## Tech Stack
- **Frontend:** React Native, Expo, TypeScript
- **Backend:** Supabase (PostgreSQL, Edge Functions, Storage, Auth)
- **AI:** Anthropic Claude Vision API
- **Infrastructure:** Vercel-adjacent serverless, APNs push notifications

## Key Engineering Challenges Solved
- Diagnosed and fixed 0-byte image uploads caused by fetch/blob incompatibility with React Native local URIs
- Resolved pg_net trigger permission issue (security definer blocking net schema access)
- Architected client-side AI calls through Edge Functions to prevent API key exposure
