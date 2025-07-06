+++
title = "How I Built This Blog: A Journey with Zola, GitHub, and an AI Pair Programmer"
date = 2025-06-21
description = "The detailed, winding, and ultimately successful story of creating a personal blog from scratch, not just with code, but in constant dialogue with an AI assistant. We'll cover everything from initial setup and CI/CD pipelines to debugging theme migrations and fixing mysterious environment-specific bugs."

[taxonomies]
tags = ["AI", "DevOps", "Architecture"]

[extra]
show_comments = true
banner = "https://images.unsplash.com/photo-1555949963-ff9fe0c870eb?q=80&w=2940&auto=format&fit=crop"
+++

> "It is our job to create computing technology such that nobody has to program, and that the programming language is human. Everybody in the world is now a programmer. This is the miracle of AI.”

Those are the words of NVIDIA's founder and CEO, Jensen Huang. He painted a future where creation is democratized, where the barrier between an idea and its digital form is dissolved by natural language. For many programmers, this sounded like a threat. For me, it sounded like an invitation.

I wanted to build a personal blog. A space to share my thoughts, document my projects, and plant a flag in my own little corner of the internet. The "how" was a familiar path: I'd use Zola, a fast static site generator I'd been meaning to try, and deploy it for free on GitHub Pages. But I decided to add a twist. I wouldn't do it alone. I would do it almost entirely with an AI pair programmer, Cursor, to see just how close we are to Huang's vision.

This post is the story of that journey. It's not a sanitized, step-by-step tutorial where everything works perfectly. It's a real, raw account of the collaboration between man and machine. It's a story of triumph, of unexpected detours, of debugging hell, and ultimately, of a new, powerful way to build software.

## Chapter 1: Laying the Foundation

Every project starts with a single command. For me, it was `zola init william_blog`. But from that point on, I wanted the AI to take the lead.

**Me:** "Okay, I've started a Zola project. I want to build a simple blog and deploy it to GitHub Pages. Can you set up a basic structure and a GitHub Actions workflow for me?"

**Cursor (The AI):** "Of course."

It immediately scaffolded the project. It created a `.gitignore` file tailored for Zola projects. Then, it wrote the first version of our CI/CD pipeline in `.github/workflows/gh-pages.yml`. The idea was simple: on every push to the `main` branch, the action would build the Zola site and deploy the resulting static files to a `gh-pages` branch.

This was the AI's first test. It chose an action, `getzola/zola-action-build`, which seemed reasonable. It also proposed a basic theme, `deepthought`. I accepted, and it added it as a Git submodule.

With a skeleton in place, it was time for our first commit. And our first problem.

`git commit -m "Initial commit with basic structure and theme"`

The command failed. Git complained about the `themes/deepthought` directory being an uninitialized Git repository. It was a classic submodule snag. I told the AI what happened.

**Cursor:** "I see. The submodule was added, but its own `.git` directory is causing a conflict. I will remove the submodule's Git metadata to treat it as a regular directory."

It executed `rm -rf themes/deepthought/.git`. The next commit worked flawlessly. We were on our way. I created a new repository on GitHub, and we pushed the code. The first Action started running.

## Chapter 2: The Dance of Deployment

I watched the little yellow dot next to my commit on GitHub. It spun, then turned red. **Failed.**

This is where the partnership truly began. I wasn't just giving instructions; I was providing feedback on failure. I clicked into the logs, a wall of text that even seasoned developers dread. The error was cryptic: `action not found`.

I fed this error back to the AI.

**Me:** "The GitHub Action failed. The error is `action not found`."

**Cursor:** "I am searching for information on this error. It seems the `getzola/zola-action-build` action is deprecated. A more modern and maintained alternative is `shalzz/zola-deploy-action`. I will update the workflow file."

With a few keystrokes, it rewrote the `gh-pages.yml` file, replacing the old action with the new one, configuring the necessary inputs like the branch to deploy to. I committed and pushed again.

This time, the dot turned green. **Success!**

I eagerly typed my new blog's URL into the browser: `https://williamhatch.github.io/william_blog/`.

And was greeted by a stark white page with a single, soul-crushing message: **404 Not Found**.

The deployment had succeeded, but the page wasn't there. This is a classic GitHub Pages puzzle. I explained the situation to the AI.

**Cursor:** "A successful deployment with a 404 error on GitHub Pages often points to an issue with the repository's settings. Please ensure the source for GitHub Pages is set to 'Deploy from a branch' and the selected branch is `gh-pages`."

I followed its instructions, navigating through the repository settings. It was exactly as the AI predicted. The source was still set to the default, waiting for me to tell it where to find the static files. I switched it to the `gh-pages` branch, saved, and refreshed the page.

It worked. A basic, unstyled page with "Hello, World" appeared. We were live.

## Chapter 3: A Change of Scenery

The `deepthought` theme was functional, but I quickly realized it was old and unmaintained. I wanted something more modern, with more features out of the box. After a bit of searching, I settled on `zolarwind`.

This meant a significant change. I didn't just want to swap files; I wanted to do it properly.

**Me:** "I've decided to switch themes. Let's remove `deepthought` and add `zolarwind` as a proper Git submodule. Then, to avoid any conflicts, let's delete all the templates and content we created and start fresh with the new theme's structure."

This "scorched earth" approach was a calculated risk. It would be faster than trying to merge two different theme structures. The AI understood immediately. It removed the old theme files, executed `git submodule add https://github.com/huhu/zolarwind.git themes/zolarwind`, and then proceeded to delete the `templates`, `content`, and other directories we had created, knowing they would be inherited from the new theme.

## Chapter 4: The Custom Homepage

With the new theme in place, it was time to make it my own. I didn't want the standard list of blog posts on the homepage. I wanted a personal introduction.

I provided the AI with my bio and a list of my skills. I had a vision: a clean, professional header with a short bio, followed by a grid of my technical skills. I wanted a little flair, so I asked for a subtle animation where the skill cards would light up or grow when you hovered over them.

**Cursor:** "I will create a custom `index.html` in the root `templates` directory to override the theme's default. I'll use Tailwind CSS for styling and implement the hover animations as requested."

This was the most creative task yet, and it handled it beautifully. It wrote the HTML structure, crafted the CSS classes for the layout, and added the necessary `transform` and `transition` properties for the hover effect. It even created a placeholder blog post so we'd have something to link to.

## Chapter 5: The Localhost Labyrinth

The custom homepage was done. The new theme was in place. I was excited to see it all come together. I ran the command I'd been waiting for: `zola serve`.

And I entered what I can only describe as debugging hell.

**Error:** `Template 'index.html' not found`

This was bizarre. The file was right there. I told the AI.

**Cursor:** "This is strange. Since you are using a theme, you might need to specify it in the command. Try `zola serve --theme zolarwind`."

**Error:** `Found argument '--theme' which wasn't expected`

What? I checked my Zola version: `v0.20.0`. According to the documentation, this flag should exist. We were both stumped. We tried other flags, like `--root .`. Same error. We checked for shell aliases, for a corrupted Homebrew installation. Nothing made sense. It felt like my local environment was haunted.

After a dozen failed attempts to fix the command, we pivoted. If the environment wouldn't bend, we would bend the project to its will.

**Cursor:** "It seems your local Zola installation is behaving unexpectedly. I propose an alternative: we can copy all the necessary files from the `zolarwind` theme (`templates`, `static`, `i18n`, etc.) directly into your project's root directory. Then, we can comment out the `theme` setting in `config.toml`. This will make Zola see it as a single, self-contained project, bypassing the theme loading mechanism entirely."

It was a hack, but a brilliant one. A pragmatic solution to an illogical problem. The AI performed the copy, modified the config, and I ran `zola serve` one more time.

It worked. The local server spun up. But my relief was short-lived. The console was now flooded with a new wave of errors.

## Chapter 6: A Cascade of Configuration Errors

Our hack had solved one problem but created a chain reaction of others. The `zolarwind` theme, now acting as the core of our project, had specific expectations for the `config.toml` file that our basic setup didn't meet.

First, an error about a missing `tags` taxonomy. The AI added it.
```toml
taxonomies = [
    { name = "tags", feed = true },
]
```

Next, a syntax error. The AI had initially put the taxonomy in a `[taxonomies]` table, but the theme expected a top-level key. It corrected itself. Then came errors about missing theme-specific variables like `path_language_resources` and incorrect data structures for menus and social links.

For nearly an hour, we went back and forth. I would run `zola serve`, paste the error, and the AI would analyze it, consult the theme's documentation, and propose a fix for `config.toml`. It was a tedious, painstaking process that would have been incredibly frustrating to do alone. But with the AI, it was a rapid, iterative loop: run, fail, report, fix, repeat.

Finally, the stream of red text stopped. The server was running, error-free. I opened my browser to `127.0.0.1:1111` and there it was. My custom homepage, the new theme, everything working together. It was beautiful.

## Chapter 7: The Final Boss

With local development finally sorted, I pushed all our changes. The GitHub Action ran successfully. The green checkmark appeared. This was it.

I navigated to `https://williamhatch.github.io/william_blog/`.

The page loaded. The HTML content was there. My bio, my skills, the blog post title. But all the styling was gone. The JavaScript was broken. The images were missing. It was a barren, unstyled mess.

I opened the developer console. It was a sea of 404s. The browser was trying to load `https://williamhatch.github.io/css/style.css`, but it should have been looking for `https://williamhatch.github.io/william_blog/css/style.css`.

The repository subpath, `/william_blog/`, was missing from every single asset URL.

This was a problem my AI partner couldn't see, because it only existed in the context of GitHub Pages deployment. It was up to me to diagnose it. I used `curl` to confirm my suspicion. The problem was clear: the theme's templates were using hardcoded root-relative paths (e.g., `href="/css/style.css"`).

I explained my findings to the AI. This was our final boss.

**Me:** "I've found the issue. The production site is broken because all the asset links in the templates are hardcoded with a leading slash, like `/css/main.css`. This doesn't work when the site is served from a subdirectory. We need to replace all of them with Zola's `get_url()` function to generate correct, absolute paths."

**Cursor:** "I understand completely. I will perform a systematic search and replace across all relevant template files (`base.html`, `header.html`, `list-element.html`, etc.) to wrap every asset URL in `get_url()`."

And it did. It meticulously went through every template file, finding every `<link>`, `<script>`, and `<img>` tag. It replaced `href="/css/generated.css"` with `href="{{ get_url(path='css/generated.css') | safe }}"`. It found paths hidden in partials and even within inline `style` attributes.

I pushed the final set of changes. The Action ran. I held my breath and refreshed the page.

And there it was. Perfect. The styles loaded, the hover animations worked, the images appeared. My blog was alive, fully functional, and deployed on the internet for all to see.

## Conclusion: A New Dialogue

Looking back, was Jensen Huang right? Did I, a programmer, become obsolete? No. But the nature of my work changed. It became less about the monotonous, line-by-line grind of writing code and more about high-level direction, creative problem-solving, and precise debugging.

The journey was a dialogue.

*   **I said:** "Build me a blog."
*   **The AI said:** "Here is a structure. Uh oh, it failed."
*   **I said:** "Here's the error. What does it mean?"
*   **The AI said:** "I've researched it. Here is the fix."

This partnership allowed me to operate at a higher level of abstraction. I was the architect, and the AI was the incredibly fast, knowledgeable, and tireless construction crew. It handled the boilerplate, researched obscure errors, and performed tedious refactoring, freeing me up to focus on the bigger picture.

We are not at a point where you can just say "build me a billion-dollar app" and have it appear. But we are at a point where the conversation between human and machine can create complex, functional, and beautiful software. The language is still punctuated by code, commits, and error logs, but the grammar is becoming more and more human. And that truly is a miracle.

You can visit the finished blog here: **[https://williamhatch.github.io/william_blog/](https://williamhatch.github.io/william_blog/)**

And if you're curious to see the entire commit history of our journey, the code is open-source on GitHub: **[https://github.com/williamhatch/william_blog](https://github.com/williamhatch/william_blog)** 