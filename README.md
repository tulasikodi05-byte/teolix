# 🎬 TEOLIX — Train Your Eyes. Brain. Tongue.

> English fluency app for Indian learners. Watch real videos → describe what you see → get AI feedback. Trains your brain to THINK in English, not translate.

**Live App:** https://teolix.vercel.app/teolix.html  
**Admin Panel:** https://teolix.vercel.app/teolix-admin.html  
**GitHub:** https://github.com/tulasikodi05-byte/teolix  
**Owner:** tulasikodi05@gmail.com  

---

## 🧠 PRODUCT VISION

Target user: 24-year-old Indian working professional (IT, office jobs) who knows English but freezes when speaking — especially in client calls, meetings, presentations.

Core insight: The problem isn't vocabulary or grammar. It's the 0.5 second translation delay (Telugu/Hindi → English). Teolix removes that delay by training the brain to think visually in English.

Core loop: Watch 8-second video → describe what you see → real AI feedback → see the gap between your answer and fluent answer → want to improve → come back tomorrow.

---

## 🏗️ TECH STACK

| Layer | Tech | Details |
|-------|------|---------|
| Frontend | HTML/CSS/JS | Single file — teolix.html |
| Hosting | Vercel | Auto deploys from GitHub |
| Database | Supabase | PostgreSQL + Auth |
| Auth | Google OAuth | Via Supabase Auth |
| AI Feedback | Groq (llama-3.1-8b-instant) | Via Supabase Edge Function |
| Translation | MyMemory API | Free, no key needed |
| QR Code | qrcodejs (cdnjs) | Pure JS, no API |
| Payments | Razorpay | Code exists, key needed |
| Videos | Pexels API | Admin panel searches |

---

## 🔑 CREDENTIALS & KEYS

### Supabase
```
URL:  https://aqcglzjduycppamytrgn.supabase.co
KEY:  sb_publishable_PCMZQT9DcdafSdfz0SL6kg_0p0UPPZ9
```

### Groq AI
```
Stored as Supabase Edge Function Secret: GROQ_API_KEY
Edge Function name: get-feedback
Model: llama-3.1-8b-instant
Free tier: 14,400 requests/day
Console: console.groq.com
```

### Pexels (for admin video search)
```
Entered manually in admin panel UI
Get from: pexels.com/api
```

### Razorpay
```
NOT yet configured — Phase 3
Pricing: ₹299/month or ₹999/year
FREE_LIMIT = 5 scenes before paywall
```

---

## 🗄️ SUPABASE DATABASE TABLES

### `scenes`
Main content table. Videos for describing.
```
id, title, video_url, thumbnail, category, 
level, duration, model_answer, is_active, created_at
```

### `user_profiles`
One row per user.
```
id (= auth.users.id), name, avatar_url,
streak, last_active, streak_freeze, level_history
```

### `vocab_notes`
Words user saves from feedback.
```
id, user_id, scene_id, words (JSON array), created_at
```

### `mcq_scenes`
MCQ Challenge questions (3 questions, video-based).
```
id, video_url, thumbnail, question, 
option_a, option_b, option_c, option_d,
correct_option, order_num
```

### `game_questions`
Game mode questions (7 levels × 8+ questions).
```
id, video_url, thumbnail, question,
option_a, option_b, option_c, option_d,
correct_option, level, order_num, type
```

### `user_responses` ← NEW
Saves every user answer for growth tracking.
```
id, user_id, scene_id, answer_text, score, created_at
```

### `user_sessions` ← NEW
Tracks daily usage for activity grid.
```
id, user_id, date, minutes_spent, scenes_completed
```

---

## 📱 APP FEATURES (BUILT)

### Landing Page (Guest)
- Hero: "Watch. Think. Describe."
- Before/After comparison cards
- How it works section
- Testimonials
- QR code (top nav button + bottom-right auto)
- Mobile: "Works on mobile — no download! →" button

### Personalized Dashboard (Logged In)
- Good morning/afternoon/evening greeting
- Streak badge
- Continue training button (shows last score)
- 7-day activity grid
- Stats: total scenes, minutes, words saved
- Quick actions: Train, Progress, Vocab, Game

### App Player
- Category tabs: Home, Classroom, Market, Park, Office, Restaurant
- Level system (Level 1, 2, 3... per category)
- Video player with Watch Again
- Hint words (clues in native language)
- Describe textarea
- ✨ Get Feedback button → Groq AI

### AI Feedback (Groq)
- Score /10
- Warm encouragement message
- Specific improvement tip
- Gap motivation message
- Your answer vs Fluent speaker answer
- Vocab section with translations
- Save to My Vocab button
- Prev/Next scene navigation

### Streak System
- Daily streak counter
- Saves to Supabase (logged in users)
- localStorage fallback (guests)
- Streak freeze feature
- Milestone celebrations (day 1, 3, 7, 30...)
- Streak warning if about to break

### My Vocab
- Saves words from model answers
- Shows word + translation in native language
- Stored in Supabase vocab_notes table
- Accessible from app nav

### Profile Page
- Avatar + name
- Stats: streak, mins trained, scenes done, words saved
- 30-day activity grid (GitHub-style)
- Brain Growth: Day 1 answer vs Latest answer
- Word count comparison

### Game Mode
- 7 levels of MCQ questions
- Video-based questions
- Points system (visual only — not saved yet)
- Level unlock progression
- Score screen with emoji

### MCQ Challenge
- 3 video-based MCQ questions
- Separate from Game Mode

### QR System
- Top nav: "Works on mobile — no download!" → click → QR popup
- Bottom right: auto-shows QR → cancel → collapses to pill
- qrcodejs library (pure JS, no API)
- Hidden on mobile (not needed)

### Add to Home Screen
- Android: beforeinstallprompt banner (after 30s)
- iOS: "Tap Share → Add to Home Screen" (after 30s)

### Onboarding
- Name input
- Native language selection (saves as CODE: te/hi/ta/kn/ml/bn/mr)
- Score screen
- Congrats screen

### Login / Auth
- Google OAuth via Supabase
- Login modal with skip option
- Profile menu (avatar, streak, sign out)
- Signup nudge after scene 2 (guests)

### Paywall
- Triggers after 5 free scenes
- ₹299/month or ₹999/year plans
- Razorpay integration (code ready, key needed)

---

## 🖥️ ADMIN PANEL FEATURES

URL: teolix.vercel.app/teolix-admin.html

### Sidebar Sections
- 🔍 Search Videos (Pexels search → add to DB)
- 🧠 MCQ Challenge (add/edit/delete MCQ questions)
- 🎮 Game Questions (add/edit/delete game questions)
- 🎬 All Scenes (view all with edit/delete)
- ❤️ Wishlist (save videos for later)
- 🏠 Home / 🏫 Classroom / 🛒 Market / 🌳 Park / 💼 Office / 🍽️ Restaurant (category views)

### Features
- Pexels video search + preview
- Add scene with: title, model answer, category, level
- Edit any scene (title, model answer, category, level)
- Delete any scene
- Edit MCQ questions (all 4 options + correct answer)
- Edit Game questions (options + correct + level)
- Wishlist: ❤️ save videos to check later
- Stats: total scenes, categories

---

## 🌍 MULTILINGUAL SUPPORT

Languages supported (saved as code):
```
Telugu  → te
Hindi   → hi  
Tamil   → ta
Kannada → kn
Malayalam → ml
Bengali → bn
Marathi → mr
Other   → hi (default)
```

Translation API: MyMemory (free, no key)
Used for: Hint word clues, Vocab translations

---

## 📊 TRACKING SYSTEM (NEW)

Every time user completes a scene:
1. `saveUserResponse()` → saves answer + score to `user_responses`
2. `trackSceneCompleted()` → increments `user_sessions.scenes_completed`
3. `saveSessionTime()` → saves minutes on page unload

Used in: Profile page growth comparison, Dashboard activity grid

---

## ⏳ PENDING / TODO

### High Priority
- [ ] Deploy Groq Edge Function (`get-feedback`) in Supabase
- [ ] Test Google login end-to-end on Vercel
- [ ] Add 8+ game questions per level (~56 MCQ total)
- [ ] Run SQL: `alter table user_profiles add column if not exists streak_freeze integer default 0;`

### Medium Priority  
- [ ] Points system (save to DB, unlock levels at 100 points)
- [ ] Speed rounds feature (10 second describe challenge)
- [ ] Voice describe feature (speak instead of type)
- [ ] 30-day journey spine (Day 1-10 describe, 11-20 speed, 21-30 voice)
- [ ] Mobile bottom nav bar (Profile + Upgrade at bottom)
- [ ] Before vs After Day 30 moment

### Business
- [ ] Razorpay payment integration (₹299/month, ₹999/year)
- [ ] Corporate/B2B version for IT company freshers
- [ ] Email/push streak reminders
- [ ] Phone OTP login (needs Twilio)

### Content
- [ ] 50+ scenes across all categories
- [ ] 56 game questions (8 per level × 7 levels)
- [ ] 3 MCQ challenge questions

---

## 🚀 HOW TO DEPLOY

1. Edit `teolix.html` or `teolix-admin.html`
2. Upload to GitHub: `github.com/tulasikodi05-byte/teolix`
3. Vercel auto-deploys in ~30 seconds
4. Live at: `teolix.vercel.app/teolix.html`

**NEVER test from local file:// — always use teolix.vercel.app**

---

## 💡 IF STARTING A NEW CLAUDE SESSION

Tell Claude:
> "Read my GitHub repo at github.com/tulasikodi05-byte/teolix and the README.md — then continue building Teolix. I am the founder. Here is what I need next: [your request]"

Claude will read this README and understand everything instantly.

---

## 📁 FILES

```
teolix.html          → Main user app (all-in-one)
teolix-admin.html    → Admin panel
README.md            → This file (your insurance!)
create_tables.sql    → SQL to create DB tables
index.ts             → Supabase Edge Function (get-feedback)
```

---

*Built with Claude AI — Session 1 through 7+*  
*Started: March 2026*
