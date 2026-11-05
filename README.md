# Mug Smarter auto-posting

Same setup as list.sg (see the Claude Doc "Automated Instagram Posting with GitHub Actions"), with these changes:

- **Posts by date, daily.** Each item's `post.json` has `post_on` (from "Post on:" in `caption.md`). A run posts every approved, unposted item dated today (SGT), in folder order, so the three pins all go out on Sun 8 Nov.
- **Missed days don't auto-post.** They make the run fail (GitHub emails you). Run the workflow with `catch_up` ticked to post them.
- **Single-image posts** (the cheatsheets) are supported.
- **Alt text** from `caption.md` is set on every image.

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

cron-job.org calls the workflow at 20:30 SGT **every day** (parents scroll after dinner and homework) (URL, method and headers as in the doc).
GitHub's own schedule runs hourly at 20:30, 21:30 and 22:30 SGT as a backup.
Running more than once a day is safe, because posted items are skipped.

Pinning is manual. After the 8 Nov posts go out, pin them in the order 3, then 2, then 1.
