
# LeetCode Tracker

**An official college portal for tracking student problem-solving progress at Dr. N.G.P. Institute of Technology.**

Built by the Department of Computer Science and Engineering in three months, the portal gives institutional staff a shared view of student LeetCode activity, class progress, assignments, and streaks. The production portal is live.

## Overview

LeetCode Tracker provides role-aware dashboards for institutional staff and student-facing progress views. Staff access is scoped by role and class assignments, with HOD and Dean access supporting broader department or institution-level views.

### Capabilities

- Staff dashboards for HODs, Deans, teachers, tutors, and class advisors
- Class and student progress views, including solved-problem totals and streaks
- Activity and task-management pages, including class-specific task details
- Student directory, staff details, class views, and account settings
- Supabase authentication and data access
- Student data export utilities and dashboard visualizations

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

## Technology

- [Next.js](https://nextjs.org/) 16 with the App Router
- [React](https://react.dev/) 19 and [TypeScript](https://www.typescriptlang.org/)
- [Supabase](https://supabase.com/) for authentication and database access
- [Tailwind CSS](https://tailwindcss.com/) 4 for styling
- Recharts and Framer Motion for visualizations and motion
- xlsx and jsPDF for export support

## Run Locally

### Requirements

- Node.js compatible with the installed Next.js version
- npm
- A Supabase project with the portal's required schema and staff accounts

### Setup

1. Install dependencies:

	 ```bash
	 npm install
	 ```

2. Create `.env.local` in the project root and add your Supabase project values:

	 ```dotenv
	 NEXT_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT_ID.supabase.co
	 NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=YOUR_SUPABASE_PUBLISHABLE_KEY
	 ```

	 Use the publishable key intended for client-side use. Do not expose a Supabase service-role key through a `NEXT_PUBLIC_` variable.

3. Start the development server:

	 ```bash
	 npm run dev
	 ```

4. Open [http://localhost:3000](http://localhost:3000).

### Available Commands

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
	login/                  Sign-in page
components/
	layout/                 Dashboard shell, sidebar, and top navigation
	staff-dashboard/        Class, settings, and leaderboard panels
	ui/                     Shared interface components
lib/
	hooks/                  Staff class data hook
	supabase/               Browser and server Supabase clients
	export-utils.ts         Student export helpers
	mock-data.ts            Sample dashboard data
proxy.ts                  Authentication and role-aware route access
types/                    Shared application types
```

## Data and Deployment Notes

- The staff dashboard and Supabase client utilities use the configured Supabase project. Access depends on valid project credentials, the expected database tables, and correctly linked staff accounts.
- The admin overview currently uses sample data from `lib/mock-data.ts`; its displayed metrics should not be treated as live institutional totals.
- `POST /api/sync` currently returns a simulated success response. Fetching LeetCode data and writing refreshed summaries to Supabase are not implemented in that endpoint yet.
- The production portal is live. Configure the same Supabase environment variables in the deployment environment; the live URL is not included here.

## Contributing

Before opening a change, run `npm run lint` and `npm run build`. Keep credentials and student information out of source control, screenshots, and issue reports.

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a new font family for Vercel.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.
