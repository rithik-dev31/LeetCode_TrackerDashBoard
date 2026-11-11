# LeetCode Tracker

**Official student problem-solving progress portal for Dr. N.G.P. Institute of Technology.**

Built by the Department of Computer Science and Engineering in three months, LeetCode Tracker gives institutional staff a shared view of student LeetCode activity, class progress, assignments, and streaks. The production portal is live.

## Table of Contents

- [Overview](#overview)
- [Application Map](#application-map)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Known Limitations](#known-limitations)
- [Deployment](#deployment)
- [Contributing](#contributing)

## Overview

The portal provides role-aware dashboards for institutional staff and student-facing progress views. Staff access is scoped by role and class assignment, with HOD and Dean roles supporting broader department- and institution-level views.

### Features

- Staff dashboards for HODs, Deans, teachers, tutors, and class advisors
- Class and student progress views, including solved-problem totals and streaks
- Activity and task management, including class-specific task details
- Student directory, staff details, class views, and account settings
- Supabase authentication and data access
- Student data export (Excel and PDF) and dashboard visualizations

## Application Map

| Area | Route | Purpose |
| --- | --- | --- |
| Home | `/` | Portal introduction and sign-in entry point |
| Sign in | `/login` | Staff authentication |
| Staff dashboard | `/dashboard/staff` | Role-scoped overview of students, classes, and activity |
| Activities | `/dashboard/staff/activity` | Staff activity workflow |
| Tasks | `/dashboard/staff/task` | Task management |
| Task details | `/dashboard/staff/task-detail-class/[taskId]/[classId]` | Task progress for a class |
| Student directory | `/dashboard/staff/students` | Student list for authorized staff |
| Staff details | `/dashboard/staff/staff-detail` | Staff information for authorized roles |
| Student profile | `/dashboard/student/[id]` | Individual student progress |
| Class overview | `/dashboard/class/[id]` | Class-level progress view |
| My class | `/dashboard/class/my-class` | Assigned class view |
| Settings | `/dashboard/settings` | Account settings |
| Admin overview | `/dashboard/admin` | Institutional overview |

## Architecture

```mermaid
flowchart TD
	subgraph experience["Portal routes"]
		home["Home and sign in"]
		staff["Staff dashboard"]
		activity["Activities"]
		tasks["Task management and class task details"]
		directory["Student directory and staff details"]
		class["Class views"]
		student["Student profile"]
		admin["Admin overview"]
		settings["Settings"]
	end

	subgraph ui["Shared interface"]
		shell["DashboardLayout"]
		sidebar["Sidebar"]
		topnav["TopNav"]
		panels["Staff dashboard panels"]
		components["Shared UI components"]
	end

	subgraph services["Data and services"]
		proxy["proxy.ts: session and role-aware route access"]
		hook["use-staff-classes hook"]
		supabase["Supabase Auth and database"]
		sync["POST /api/sync: simulated sync response"]
		mock["Mock dashboard data"]
		export["Student export utilities"]
	end

	home --> proxy
	staff --> shell
	activity --> shell
	tasks --> shell
	directory --> shell
	class --> shell
	student --> shell
	admin --> shell
	settings --> shell
	shell --> sidebar
	shell --> topnav
	shell --> panels
	shell --> components
	proxy --> supabase
	staff --> supabase
	hook --> supabase
	activity --> supabase
	tasks --> supabase
	panels --> export
	admin -. "uses sample data" .-> mock
	sync -. "not yet connected to LeetCode or database writes" .-> supabase
```

## Technology Stack

| Layer | Technology |
| --- | --- |
| Framework | [Next.js](https://nextjs.org/) 16 (App Router) |
| UI | [React](https://react.dev/) 19, [TypeScript](https://www.typescriptlang.org/) |
| Backend | [Supabase](https://supabase.com/) (authentication and database) |
| Styling | [Tailwind CSS](https://tailwindcss.com/) 4 |
| Visualization and motion | Recharts, Framer Motion |
| Export | xlsx, jsPDF |
| Fonts | [Geist](https://vercel.com/font) via `next/font` |

## Getting Started

### Prerequisites

- Node.js compatible with the installed Next.js version
- npm
- A Supabase project with the portal's required schema and linked staff accounts

### Installation

1. **Install dependencies**

   ```bash
   npm install
   ```

2. **Configure environment variables**

   Create `.env.local` in the project root:

   ```dotenv
   NEXT_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT_ID.supabase.co
   NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=YOUR_SUPABASE_PUBLISHABLE_KEY
   ```

   > Use the publishable key intended for client-side use. Never expose a Supabase service-role key through a `NEXT_PUBLIC_` variable.

3. **Start the development server**

   ```bash
   npm run dev
   ```

4. Open [http://localhost:3000](http://localhost:3000). The app reloads as you edit files (for example, `app/page.tsx`).

### Available Scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Start the local development server |
| `npm run build` | Create a production build |
| `npm run start` | Serve the production build |
| `npm run lint` | Run ESLint |

## Project Structure

```text
app/
	api/sync/              LeetCode sync endpoint
	dashboard/             Staff, student, class, admin, and settings routes
	login/                 Sign-in page
components/
	layout/                Dashboard shell, sidebar, and top navigation
	staff-dashboard/       Class, settings, and leaderboard panels
	ui/                    Shared interface components
lib/
	hooks/                 Staff class data hook
	supabase/              Browser and server Supabase clients
	export-utils.ts        Student export helpers
	mock-data.ts           Sample dashboard data
proxy.ts                 Authentication and role-aware route access
types/                   Shared application types
```

## Known Limitations

- **Admin overview uses sample data.** `/dashboard/admin` reads from `lib/mock-data.ts`; its metrics are not live institutional totals.
- **LeetCode sync is a stub.** `POST /api/sync` returns a simulated success response. Fetching LeetCode data and writing refreshed summaries to Supabase are not yet implemented.
- **Supabase dependency.** The staff dashboard requires valid project credentials, the expected database tables, and correctly linked staff accounts.

## Deployment

The production portal is live (the URL is intentionally not listed here). To deploy, configure the same Supabase environment variables listed above in your hosting environment. The project is a standard Next.js app and can be hosted on [Vercel](https://vercel.com/) or any platform that supports Next.js. See the [Next.js deployment docs](https://nextjs.org/docs/app/building-your-application/deploying) for details.

## Contributing

1. Make your changes on a branch.
2. Run `npm run lint` and `npm run build` before opening a change.
3. Keep credentials and student information out of source control, screenshots, and issue reports.
