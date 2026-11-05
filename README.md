# Mug Smarter auto-posting

Same setup as list.sg (see the Claude Doc "Automated Instagram Posting with GitHub Actions"), with these changes:

- **Posts by date, daily.** Each item's `post.json` has `post_on` (from "Post on:" in `caption.md`). A run posts every approved, unposted item dated today (SGT), in folder order, so the three pins all go out together (Thu 5 Nov).
- **A missed post blocks the ones after it.** The run posts nothing and fails, so GitHub emails you. Run the workflow with `catch_up` ticked to post everything in date order.
- **Single-image posts** (the cheatsheets) are supported.
- **Alt text** from `caption.md` is set on every image.
- **Per-post time.** `**Time:** HH:MM` in `caption.md` (SGT); default 20:30. Reels go out at 12:30.
- **Reels.** A post folder with `reel.mp4` (instead of `slides/`) queues as a Reel; `**Cover:** 1.8 s` picks the cover frame.

## Add or refresh posts

```sh
cd ig-queue
python3 queue_add.py ../posts/day15_*            # new post(s), queued as approved
python3 queue_add.py --update ../posts/day1[5-9]_* # after re-exporting slides or editing the caption
python3 publish.py status
git add queue && git commit -m "Queue days 15–21" && git pull --rebase && git push
```

Only queue posts that have been fact-checked: pushed slides are public on GitHub Pages at once.
To hold one back, set `"approved": false` in its `post.json` and push.

## Schedule

cron-job.org calls the workflow at **12:30 and 20:30 SGT every day** (hours 12 and 20, minute 30; parents scroll at lunch and after dinner) (URL, method and headers as in the doc).
GitHub's own schedule runs hourly after each (12:30–14:30, 20:30–22:30 SGT) as a backup, posting only items due in the last 3 hours.
Running more than once a day is safe, because posted items are skipped.

Pinning is manual. After the pins go out (Thu 5 Nov), pin them in the order 3, then 2, then 1.
