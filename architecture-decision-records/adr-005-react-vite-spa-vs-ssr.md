# ADR-005: React SPA vs. Server-Side Rendering (SSR)

## The Decision
I chose to build frontend interfaces (CogniTrace, SmartFood, Ajaysinh NGO Platform) as client-side **React Single Page Applications (SPAs)** built with **Vite**, rejecting Server-Side Rendering (Next.js/SSR) architectures.

---

## Context & Trade-offs

My projects require highly interactive dashboards. For example, CogniTrace displays real-time coordinates, charts, and telemetry maps, which update continuously.

### Why React SPA
*   **Decoupled Architecture**: The frontend compile output consists of static files (HTML, JS, CSS) hosted on a CDN (like Netlify or S3). The frontend communicates with backend APIs strictly via JSON endpoints, simplifying testing, scaling, and deployments.
*   **Rich Client Interaction**: Dashboards need to handle continuous UI state changes (filtering lists, updating telemetry charts) without requiring page reloads or server-side renders.
*   **Hosting Simplicity**: Storing static files on a CDN costs almost nothing and can scale to millions of requests without managing server clusters.

### Why not Next.js / SSR
*   Next.js/SSR requires a running Node.js server to render pages dynamically. This increases hosting costs and setup complexity. Since these dashboards are secured behind login portals, the SEO benefits of pre-rendered HTML on the server are not required.

---

## What I Would Choose Today
For dashboard-centric projects, I would still use **React SPAs** with Vite.

---

## What I Would Choose If Starting Again
If rebuilding the NGO Platform, I would use a hybrid model. I would build the public-facing landing page (which requires SEO to rank on Google search) using a static site generator (like Astro), while keeping the donation checkout portal and volunteer administration pages as a decoupled client-side SPA.

---

## What Still Makes Me Uncomfortable
**Initial Bundle Size**: As applications grow, importing large visualization libraries (like Recharts or Leaflet) increases the size of the JavaScript bundle downloaded by the user. If the client has a slow network connection, they may experience initial page load delays before the React app boots. I have to manage this by configuring dynamic code-splitting (`React.lazy()`).
