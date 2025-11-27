<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Sora · AI Video Generation</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root {
      --bg: #050816;
      --bg-alt: #070b19;
      --accent: #6c5ce7;
      --accent-soft: rgba(108, 92, 231, 0.18);
      --accent-2: #00cec9;
      --text-main: #f5f7ff;
      --text-muted: #a3aed0;
      --border-subtle: rgba(255, 255, 255, 0.06);
      --radius-lg: 18px;
      --radius-xl: 24px;
      --shadow-soft: 0 24px 40px rgba(0,0,0,0.65);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "SF Pro Text",
      "Segoe UI", sans-serif;
      background: radial-gradient(circle at top, #111827 0, #050816 42%, #020412 100%);
      color: var(--text-main);
      line-height: 1.6;
      -webkit-font-smoothing: antialiased;
    }

    a {
      color: inherit;
      text-decoration: none;
    }

    .page {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    .nav {
      position: sticky;
      top: 0;
      z-index: 40;
      backdrop-filter: blur(18px);
      background: linear-gradient(to bottom,
        rgba(3, 7, 18, 0.9),
        rgba(3, 7, 18, 0.4),
        transparent
      );
      border-bottom: 1px solid rgba(148, 163, 184, 0.18);
    }

    .nav-inner {
      max-width: 1120px;
      margin: 0 auto;
      padding: 14px 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 24px;
    }

    .logo {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .logo-badge {
      width: 32px;
      height: 32px;
      border-radius: 999px;
      background: radial-gradient(circle at 20% 20%, #ffeaa7 0, #fd79a8 32%, #6c5ce7 80%);
      box-shadow: 0 0 24px rgba(108, 92, 231, 0.8);
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      font-size: 17px;
      color: #050816;
    }

    .logo-text-main {
      font-weight: 600;
      letter-spacing: 0.12em;
      font-size: 13px;
      text-transform: uppercase;
      color: #e5e7eb;
    }

    .logo-text-sub {
      font-size: 11px;
      color: var(--text-muted);
    }

    .nav-links {
      display: flex;
      gap: 20px;
      font-size: 14px;
      color: var(--text-muted);
    }

    .nav-links a {
      padding: 6px 0;
      position: relative;
    }

    .nav-links a::after {
      content: "";
      position: absolute;
      left: 0;
      bottom: -3px;
      width: 0;
      height: 2px;
      border-radius: 999px;
      background: linear-gradient(90deg, var(--accent), var(--accent-2));
      transition: width 0.2s ease-out;
    }

    .nav-links a:hover::after {
      width: 100%;
    }

    .nav-cta {
      display: flex;
      gap: 10px;
      align-items: center;
    }

    .btn {
      border-radius: 999px;
      padding: 8px 16px;
      font-size: 13px;
      border: 1px solid transparent;
      cursor: pointer;
      transition: transform 0.12s ease-out, box-shadow 0.12s ease-out, background 0.12s ease-out, border-color 0.12s ease-out;
      white-space: nowrap;
    }

    .btn-outline {
      background: rgba(15, 23, 42, 0.8);
      border-color: rgba(148, 163, 184, 0.35);
      color: #e5e7eb;
    }

    .btn-outline:hover {
      background: rgba(15, 23, 42, 1);
      transform: translateY(-1px);
      box-shadow: 0 12px 20px rgba(15, 23, 42, 0.65);
    }

    .btn-primary {
      background: radial-gradient(circle at 10% 0, #ffeaa7 0, #fd79a8 20%, #6c5ce7 60%, #00cec9 100%);
      color: #050816;
      font-weight: 600;
      box-shadow: 0 14px 28px rgba(108, 92, 231, 0.65);
      border-color: transparent;
    }

    .btn-primary:hover {
      transform: translateY(-1px) scale(1.01);
      box-shadow: 0 20px 32px rgba(108, 92, 231, 0.85);
    }

    main {
      flex: 1;
    }

    .hero {
      max-width: 1120px;
      margin: 0 auto;
      padding: 40px 20px 72px;
      display: grid;
      grid-template-columns: minmax(0, 1.15fr) minmax(0, 1fr);
      gap: 40px;
      align-items: center;
    }

    @media (max-width: 900px) {
      .hero {
        grid-template-columns: minmax(0, 1fr);
      }
      .nav-inner {
        padding-inline: 16px;
      }
      .nav-links {
        display: none;
      }
      .nav-cta {
        gap: 6px;
      }
    }

    @media (max-width: 640px) {
      .hero {
        padding-top: 28px;
      }
      .hero h1 {
        font-size: 30px !important;
      }
    }

    .eyebrow {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 3px 10px 3px 3px;
      border-radius: 999px;
      background: linear-gradient(to right,
        rgba(108, 92, 231, 0.3),
        rgba(0, 206, 201, 0.02)
      );
      border: 1px solid rgba(129, 140, 248, 0.45);
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 0.14em;
      color: #c7d2fe;
      margin-bottom: 10px;
    }

    .eyebrow-pill {
      width: 20px;
      height: 20px;
      border-radius: 999px;
      background: radial-gradient(circle at 20% 10%, #ffeaa7 0, #fd79a8 40%, #6c5ce7 90%);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 11px;
      color: #020617;
      box-shadow: 0 0 12px rgba(248, 250, 252, 0.8);
    }

    .hero h1 {
      font-size: 36px;
      line-height: 1.15;
      margin-bottom: 16px;
      letter-spacing: -0.03em;
    }

    .hero h1 span.gradient {
      background: radial-gradient(circle at 10% 0, #ffeaa7 0, #fd79a8 30%, #a855f7 65%, #22d3ee 100%);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    .hero-sub {
      color: var(--text-muted);
      font-size: 14px;
      max-width: 460px;
      margin-bottom: 20px;
    }

    .hero-sub strong {
      color: #e5e7eb;
      font-weight: 500;
    }

    .hero-quicklist {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 20px;
      font-size: 12px;
    }

    .hero-quicklist span {
      padding: 5px 10px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.4);
      background: linear-gradient(to bottom right,
        rgba(15, 23, 42, 0.95),
        rgba(15, 23, 42, 0.8)
      );
      color: var(--text-muted);
    }

    .hero-quicklist span b {
      color: #e5e7eb;
      font-weight: 600;
    }

    .hero-cta-row {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      align-items: center;
      margin-bottom: 12px;
    }

    .disclaimer {
      font-size: 11px;
      color: var(--text-muted);
      max-width: 420px;
    }

    .hero-right {
      position: relative;
    }

    .hero-card {
      position: relative;
      border-radius: var(--radius-xl);
      padding: 18px 18px 16px;
      background:
        radial-gradient(circle at 0 0, rgba(248, 250, 252, 0.06), transparent 40%),
        radial-gradient(circle at 100% 0, rgba(129, 140, 248, 0.2), transparent 45%),
        radial-gradient(circle at 0 100%, rgba(45, 212, 191, 0.16), transparent 40%),
        linear-gradient(135deg, #020617, #020617 40%, #020617 100%);
      box-shadow: var(--shadow-soft);
      border: 1px solid rgba(148, 163, 184, 0.3);
      overflow: hidden;
    }

    .hero-card-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 12px;
    }

    .hero-card-header-left {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .hero-card-avatar {
      width: 34px;
      height: 34px;
      border-radius: 999px;
      background: radial-gradient(circle at 20% 0, #ffeaa7 0, #fd79a8 40%, #6c5ce7 90%);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      color: #020617;
      font-weight: 700;
      box-shadow: 0 0 10px rgba(251, 113, 133, 0.8);
    }

    .hero-card-title {
      font-size: 13px;
      font-weight: 600;
    }

    .hero-card-meta {
      font-size: 11px;
      color: var(--text-muted);
    }

    .hero-card-pill {
      font-size: 11px;
      padding: 5px 10px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px solid rgba(148, 163, 184, 0.5);
      color: #e5e7eb;
    }

    .hero-fake-video {
      position: relative;
      border-radius: 16px;
      padding: 12px;
      background: radial-gradient(circle at 0 0, rgba(248, 250, 252, 0.16), transparent 40%),
                  radial-gradient(circle at 100% 100%, rgba(56, 189, 248, 0.26), transparent 40%),
                  #020617;
      border: 1px solid rgba(148, 163, 184, 0.4);
      overflow: hidden;
      margin-bottom: 12px;
    }

    .hero-fake-video-inner {
      border-radius: 12px;
      height: 180px;
      background-image:
        radial-gradient(circle at 10% 20%, rgba(251, 191, 36, 0.8), transparent 60%),
        radial-gradient(circle at 80% 0%, rgba(248, 113, 113, 0.9), transparent 60%),
        radial-gradient(circle at 0% 100%, rgba(56, 189, 248, 0.85), transparent 60%),
        radial-gradient(circle at 100% 100%, rgba(94, 234, 212, 0.8), transparent 60%);
      position: relative;
      display: flex;
      align-items: flex-end;
      justify-content: space-between;
      padding: 16px;
      color: #020617;
    }

    .hero-fake-video-caption {
      background: rgba(15, 23, 42, 0.86);
      color: #e5e7eb;
      padding: 8px 10px;
      border-radius: 10px;
      font-size: 11px;
      max-width: 70%;
      box-shadow: 0 8px 20px rgba(15, 23, 42, 0.7);
    }

    .hero-fake-video-caption b {
      color: #f97316;
      font-weight: 600;
    }

    .hero-fake-video-pill {
      padding: 6px 10px;
      border-radius: 999px;
      background: rgba(15, 23, 42, 0.9);
      color: #e5e7eb;
      font-size: 11px;
      border: 1px solid rgba(148, 163, 184, 0.4);
    }

    .hero-card-footer {
      display: flex;
      justify-content: space-between;
      font-size: 11px;
      color: var(--text-muted);
      gap: 12px;
      flex-wrap: wrap;
    }

    .hero-card-footer span {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      white-space: nowrap;
    }

    .hero-card-footer span::before {
      content: "•";
      color: rgba(148, 163, 184, 0.9);
    }

    .glow-orbit {
      position: absolute;
      inset: -40px;
      border-radius: 999px;
      border: 1px solid rgba(94, 234, 212, 0.06);
      filter: blur(5px);
      pointer-events: none;
    }

    /* Sections */

    .section {
      max-width: 1120px;
      margin: 0 auto;
      padding: 0 20px 70px;
    }

    .section-header {
      margin-bottom: 22px;
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
      gap: 12px;
      flex-wrap: wrap;
    }

    .section-title {
      font-size: 20px;
      letter-spacing: -0.02em;
    }

    .section-subtitle {
      font-size: 13px;
      color: var(--text-muted);
      max-width: 340px;
    }

    .pill-row {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .pill-row span {
      font-size: 11px;
      padding: 5px 10px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.4);
      color: var(--text-muted);
      background: rgba(15, 23, 42, 0.9);
    }

    .feature-grid {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 16px;
    }

    @media (max-width: 900px) {
      .feature-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr));
      }
    }

    @media (max-width: 640px) {
      .feature-grid {
        grid-template-columns: minmax(0, 1fr);
      }
    }

    .card {
      border-radius: var(--radius-lg);
      padding: 14px 14px 16px;
      background: linear-gradient(135deg, rgba(15, 23, 42, 0.98), rgba(15, 23, 42, 0.96));
      border: 1px solid var(--border-subtle);
      box-shadow: 0 14px 30px rgba(15, 23, 42, 0.85);
      position: relative;
      overflow: hidden;
    }

    .card-label {
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 0.14em;
      color: #a5b4fc;
      margin-bottom: 6px;
    }

    .card-title {
      font-size: 14px;
      font-weight: 600;
      margin-bottom: 6px;
    }

    .card-body {
      font-size: 12px;
      color: var(--text-muted);
      margin-bottom: 8px;
    }

    .card-tag-row {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      font-size: 10px;
    }

    .card-tag-row span {
      padding: 4px 8px;
      border-radius: 999px;
      border: 1px solid rgba(148, 163, 184, 0.4);
      background: rgba(15, 23, 42, 0.9);
    }

    .card-accent {
      position: absolute;
      inset: -80px;
      opacity: 0.06;
      background:
        radial-gradient(circle at 0 0, #facc15 0, transparent 50%),
        radial-gradient(circle at 100% 30%, #ef4444 0, transparent 50%),
        radial-gradient(circle at 0 100%, #22c55e 0, transparent 50%);
      pointer-events: none;
    }

    .two-column {
      display: grid;
      grid-template-columns: minmax(0, 1.1fr) minmax(0, 1fr);
      gap: 18px;
    }

    @media (max-width: 900px) {
      .two-column {
        grid-template-columns: minmax(0, 1fr);
      }
    }

    .callout {
      font-size: 12px;
      color: var(--text-muted);
      padding: 12px 12px;
      border-radius: var(--radius-lg);
      border: 1px dashed rgba(94, 234, 212, 0.5);
      background: rgba(15, 23, 42, 0.95);
      margin-top: 12px;
    }

    .callout b {
      color: #e5e7eb;
    }

    .table {
      width: 100%;
      border-collapse: collapse;
      font-size: 12px;
      margin-top: 4px;
    }

    .table th,
    .table td {
      padding: 6px 8px;
      border-bottom: 1px solid rgba(30, 64, 175, 0.7);
    }

    .table th {
      text-align: left;
      color: #c7d2fe;
      font-weight: 500;
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 0.08em;
    }

    .table tr:nth-child(even) td {
      background: rgba(15, 23, 42, 0.75);
    }

    .faq {
      margin-top: 14px;
      display: grid;
      gap: 10px;
    }

    .faq-item {
      border-radius: 12px;
      border: 1px solid var(--border-subtle);
      padding: 10px 12px;
      background: rgba(15, 23, 42, 0.95);
      font-size: 12px;
    }

    .faq-q {
      font-weight: 500;
      margin-bottom: 4px;
      color: #e5e7eb;
    }

    .faq-a {
      color: var(--text-muted);
    }

    footer {
      border-top: 1px solid rgba(30, 64, 175, 0.7);
      padding: 16px 20px 24px;
      font-size: 11px;
      color: var(--text-muted);
      text-align: center;
      background: radial-gradient(circle at top, #020617 0, #000 85%);
    }

    footer a {
      color: #a5b4fc;
      text-decoration: underline;
      text-decoration-style: dotted;
      text-underline-offset: 3px;
    }
  </style>
</head>
<body>
<div class="page">
  <header class="nav">
    <div class="nav-inner">
      <div class="logo">
        <div class="logo-badge">S</div>
        <div>
          <div class="logo-text-main">SORA</div>
          <div class="logo-text-sub">Concept AI video studio</div>
        </div>
      </div>
      <nav class="nav-links">
        <a href="#features">Features</a>
        <a href="#workflows">Workflows</a>
        <a href="#safety">Safety</a>
        <a href="#faq">FAQ</a>
      </nav>
      <div class="nav-cta">
        <button class="btn btn-outline">View docs</button>
        <button class="btn btn-primary">Try Sora demo</button>
      </div>
    </div>
  </header>

  <main>
    <!-- HERO -->
    <section class="hero" id="top">
      <div>
        <div class="eyebrow">
          <div class="eyebrow-pill">★</div>
          Text → Video in seconds
        </div>
        <h1>
          Turn <span class="gradient">ideas into cinematic video</span> with Sora.
        </h1>
        <p class="hero-sub">
          Sora is a concept <strong>AI video generation model</strong> that can turn
          everyday language into high-fidelity clips – complete with realistic
          motion, lighting and depth.
        </p>

        <div class="hero-quicklist">
          <span><b>Text-to-video</b> from a single prompt</span>
          <span><b>1080p</b> multi-second clips</span>
          <span><b>Consistent</b> subjects &amp; scenes</span>
        </div>

        <div class="hero-cta-row">
          <button class="btn btn-primary">Generate a sample clip</button>
          <button class="btn btn-outline">Watch overview</button>
        </div>
        <p class="disclaimer">
          This is a <strong>fan / educational landing page</strong> for Sora-style
          models. It is not an official product website.
        </p>
      </div>

      <div class="hero-right">
        <div class="hero-card">
          <div class="hero-card-header">
            <div class="hero-card-header-left">
              <div class="hero-card-avatar">▶</div>
              <div>
                <div class="hero-card-title">Prompt preview</div>
                <div class="hero-card-meta">“Night city timelapse, neon rain”</div>
              </div>
            </div>
            <div class="hero-card-pill">Concept demo</div>
          </div>

          <div class="hero-fake-video">
            <div class="hero-fake-video-inner">
              <div class="hero-fake-video-caption">
                Sora imagines a <b>cinematic pan</b> through neon-lit streets,
                with <b>physically-plausible reflections</b> on wet asphalt.
              </div>
              <div class="hero-fake-video-pill">
                12s · 1080p · 24 fps
              </div>
            </div>
          </div>

          <div class="hero-card-footer">
            <span>Generative scene layout</span>
            <span>Camera path + depth cues</span>
            <span>Motion-aware details</span>
          </div>

          <div class="glow-orbit"></div>
        </div>
      </div>
    </section>

    <!-- FEATURES -->
    <section class="section" id="features">
      <div class="section-header">
        <div>
          <h2 class="section-title">What Sora is designed to do</h2>
          <p class="section-subtitle">
            From storyboards to social content, Sora-style models aim to bridge
            the gap between a text idea and a fully animated sequence.
          </p>
        </div>
        <div class="pill-row">
          <span>Text → Video</span>
          <span>Image → Video</span>
          <span>Scene understanding</span>
        </div>
      </div>

      <div class="feature-grid">
        <article class="card">
          <div class="card-accent"></div>
          <div class="card-label">Generation</div>
          <h3 class="card-title">High-fidelity clips</h3>
          <p class="card-body">
            Sora can be prompted with rich natural language to produce videos
            with <strong>consistent characters, lighting and style</strong> across
            multiple seconds of motion.
          </p>
          <div class="card-tag-row">
            <span>Resolution upscaling</span>
            <span>Camera controls</span>
          </div>
        </article>

        <article class="card">
          <div class="card-accent"></div>
          <div class="card-label">Control</div>
          <h3 class="card-title">Prompt-driven direction</h3>
          <p class="card-body">
            Describe the <strong>scene, camera, emotion and pacing</strong>.
            Sora uses this to sketch trajectories, object interactions and
            timing.
          </p>
          <div class="card-tag-row">
            <span>Camera path hints</span>
            <span>Style keywords</span>
          </div>
        </article>

        <article class="card">
          <div class="card-accent"></div>
          <div class="card-label">Understanding</div>
          <h3 class="card-title">World &amp; physics priors</h3>
          <p class="card-body">
            The model learns patterns of <strong>real-world motion</strong> —
            gravity, fluids, cloth, crowds — to keep generated scenes coherent
            and believable.
          </p>
          <div class="card-tag-row">
            <span>Depth-aware frames</span>
            <span>Temporal consistency</span>
          </div>
        </article>
      </div>
    </section>

    <!-- WORKFLOWS -->
    <section class="section" id="workflows">
      <div class="section-header">
        <div>
          <h2 class="section-title">Example workflows</h2>
          <p class="section-subtitle">
            Combine text prompts, reference images and editing tools to build a
            complete production pipeline around Sora-like models.
          </p>
        </div>
        <div class="pill-row">
          <span>Storyboarding</span>
          <span>Concept art → motion</span>
          <span>Rapid iteration</span>
        </div>
      </div>

      <div class="two-column">
        <div class="card">
          <div class="card-label">Pipeline</div>
          <h3 class="card-title">Storyboard → Draft → Final</h3>
          <p class="card-body">
            1. Start with a text outline of each shot.<br />
            2. Generate quick low-res previews from prompts.<br />
            3. Lock timing, then re-render selected shots at higher quality.<br />
            4. Edit and composite in your usual video editor.
          </p>

          <table class="table">
            <thead>
            <tr>
              <th>Step</th>
              <th>Input</th>
              <th>Output</th>
            </tr>
            </thead>
            <tbody>
            <tr>
              <td>1. Prompt</td>
              <td>Text description</td>
              <td>Scene plan</td>
            </tr>
            <tr>
              <td>2. Preview</td>
              <td>Prompt + style</td>
              <td>Low-res clip</td>
            </tr>
            <tr>
              <td>3. Polish</td>
              <td>Chosen takes</td>
              <td>High-res video</td>
            </tr>
            </tbody>
          </table>

          <div class="callout">
            <b>Tip:</b> Keep prompts structured: <em>setting</em>,
            <em>subject</em>, <em>camera</em>, <em>motion</em>, <em>style</em>.
          </div>
        </div>

        <div class="card">
          <div class="card-label">Use cases</div>
          <h3 class="card-title">Where Sora-style models shine</h3>
          <ul class="card-body" style="list-style: disc; padding-left: 18px;">
            <li>Pre-visualising complex scenes before live shoots.</li>
            <li>Generating abstract or impossible camera moves.</li>
            <li>Quick mood pieces for pitches or treatments.</li>
            <li>Educative clips from plain-language scripts.</li>
          </ul>
          <div class="callout">
            <b>Remember:</b> final professional work still needs
            <strong>human editing, review and polishing</strong>.
          </div>
        </div>
      </div>
    </section>

    <!-- SAFETY / LIMITS -->
    <section class="section" id="safety">
      <div class="section-header">
        <div>
          <h2 class="section-title">Safety &amp; limitations</h2>
          <p class="section-subtitle">
            Any powerful generative system needs guardrails. This section
            highlights typical considerations when deploying video models.
          </p>
        </div>
        <div class="pill-row">
          <span>Safety filters</span>
          <span>Policy checks</span>
          <span>Labeling</span>
        </div>
      </div>

      <div class="two-column">
        <div class="card">
          <div class="card-label">Examples</div>
          <h3 class="card-title">Common limitations</h3>
          <p class="card-body">
            Even advanced models can:
          </p>
          <ul class="card-body" style="list-style: disc; padding-left: 18px;">
            <li>Misunderstand fine-grained physics in unusual scenes.</li>
            <li>Struggle with long, multi-shot continuity.</li>
            <li>Hallucinate text or tiny details on signs.</li>
            <li>Produce outputs that require careful human review.</li>
          </ul>
        </div>

        <div class="card">
          <div class="card-label">Good practice</div>
          <h3 class="card-title">Responsible use</h3>
          <p class="card-body">
            - Label AI-generated content when you share it publicly.<br />
            - Avoid prompts that could generate harmful or misleading media.<br />
            - Always review outputs before publishing.<br />
            - Follow the policies of the platform or provider you use.
          </p>
        </div>
      </div>
    </section>

    <!-- FAQ -->
    <section class="section" id="faq">
      <div class="section-header">
        <div>
          <h2 class="section-title">Frequently asked questions</h2>
          <p class="section-subtitle">
            A quick overview of how a Sora-style model fits into your creative
            workflow.
          </p>
        </div>
      </div>

      <div class="faq">
        <div class="faq-item">
          <div class="faq-q">Is this an official Sora website?</div>
          <div class="faq-a">
            No. This page is a <strong>concept / demo website layout</strong>
            created for learning and design practice. It is not affiliated with
            or endorsed by any official provider.
          </div>
        </div>

        <div class="faq-item">
          <div class="faq-q">Can I use this page for my own project?</div>
          <div class="faq-a">
            Yes – you can download the HTML file, customise the text, colours
            and buttons, and host it as your own landing page or portfolio
            piece.
          </div>
        </div>

        <div class="faq-item">
          <div class="faq-q">How do I turn this into a real product site?</div>
          <div class="faq-a">
            Connect the buttons (e.g. “Try Sora demo”) to your actual app or
            prototype, add a docs page, and plug in analytics / sign-up forms as
            needed.
          </div>
        </div>
      </div>
    </section>
  </main>

  <footer>
    Concept landing page for a Sora-style AI video model.  
    Customize the code to match your own tool, studio, or portfolio.
  </footer>
</div>
</body>
</html>
