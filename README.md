# job-hunter

**A Claude Code skill that searches for jobs, scores every offer against your profile, and keeps only the ones worth applying to.**

Job hunting means reading hundreds of ads to find five good ones. `job-hunter` does the reading:

- **First run:** it reads your CV, then asks a few quick questions: target roles, workload, location, languages, deal-breakers, your score threshold. It saves this as a profile you can edit.
- **Every run after that:** it collects new offers from pasted links, your job-alert emails and your country's job sites, then scores each one out of 10.
- **Offers at or above your threshold** (7/10 by default) go into a tracker, each with a cover letter drafted from your real CV facts.
- **Everything below the threshold** is remembered in one line, so it's never shown to you again.
- **Follow-ups:** it reminds you when an application has had no answer for 7 days.

It never applies, sends, logs in or fills in a form. You decide and you send.

## Install

```
/plugin marketplace add sacha-riccardo-leone/job-hunter
/plugin install job-hunter@job-hunter
```

Or copy `skills/job-hunter/` into `~/.claude/skills/`.

## Use

| Command | What it does |
|---|---|
| `/job-hunter` | Search, score, record and reply (runs setup on first use) |
| `/job-hunter <link or pasted ad>` | Score just that offer |
| `/job-hunter status` | Tracker summary and follow-ups due |
| `/job-hunter sent <company>` | Mark an application as sent |
| `/job-hunter update <company> interview` | Change an application's status |
| `/job-hunter setup` | Redo the interview |
| `/job-hunter add-source <site or URL>` | Add a job site that isn't covered (see below) |
| `/job-hunter sources` | See which sites are searched for you, and which are alert-only |

A run's reply looks like this:

```
2 new offers ≥ 7/10:
8/10 · Acme · Junior Python Developer · Lyon (hybrid): stack match, local · missing: Django depth
       letter: ~/.claude/job-hunter/letters/2026-11-25-acme.md
7/10 · Globex · Web Developer · Remote: React match · missing: 2 yrs asked
14 offers below 7 skipped · Sites skipped (blocked): none
Follow-ups due: Initech (sent 8 days ago)
```

## Add a job site that's missing

Your niche board, regional site or dream company's careers page isn't covered? Paste one of its search-results URLs:

```
/job-hunter add-source https://www.example-jobs.com/search?q=developer&city=Lyon
```

Claude tests it with your own keywords and city (5 requests max, no tricks), checks that search, dates and paging really work, and then:

- **✅ it works:** the site is added to every run from now on.
- **⚠️ it loads but hides its listings:** you're told why, and to use the site's email alert instead.
- **📧 it blocks automated reading:** you're told to set up its email alert.

Your sites live in `~/.claude/job-hunter/my-sources.md`, so plugin updates never erase them. If one stops working, the next run tells you. And if it works, Claude gives you a ready-made snippet so you can share it with everyone through an issue or pull request here.

## Tips

- **Create job-alert emails** on the big sites (LinkedIn, Indeed, and your country's main boards) and connect your mail in Claude. Alerts are the most complete source, and no site blocks them.
- **Edit the scoring** in `~/.claude/job-hunter/profile.md` until the scores match your own gut feeling.
- **Pair it with [cv-check](https://github.com/sacha-riccardo-leone/cv-check)** to review each drafted letter for clichés and missing keywords.

## Privacy and fair use

Everything stays in `~/.claude/job-hunter/` on your machine. It reads job sites only the way a visitor would. If a site blocks automated reading, it skips that site and suggests an email alert instead. It never gets around blocks, captchas or logins.

## Contributing

Job sites differ by country. Run `/job-hunter add-source` on a site from your country, then open a PR adding the snippet it gives you to `skills/job-hunter/references/sources.md`. Please include the date you tested it.

## License

MIT
