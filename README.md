# MuscleUp

A phone-first gym tracker: your full Monday–Sunday plan, plus set logging that always shows
**what you lifted last time** so you know exactly what to beat.

One HTML file. No build step, no accounts, no backend, no network calls. It works with the
phone in aeroplane mode.

**Live:** https://vansh-tayal.github.io/MuscleUp/

---

## Put it on your phone (Android / Chrome)

1. Open **https://vansh-tayal.github.io/MuscleUp/** in Chrome.
2. Tap the **⋮** menu (top right) → **Add to Home screen** → **Install**.
3. Open it from the home-screen icon. It runs full screen, with no address bar.
4. Turn on aeroplane mode and open it again — it still works. That is the point.

The first load caches the app. After that it never needs the internet again.

---

## How it works

**Train** — Mon–Sun tabs, today already selected. Each exercise card shows the machine, a
coloured primary-muscle badge, the supporting muscles, and your target sets × reps.

Tap a card and every set gets a row. Weight and reps are **pre-filled with what you did last
time**, so beating last week is: tap **+**, tap **✓**. Next to each set you get last week's
numbers and a ▲ / ▼ / = against them.

**Stats** — days trained, working sets, total volume, and a bar per muscle group so a gap is
impossible to miss. Tap any exercise for its full history: every past session, a chart of your
top set over time, and your personal best.

**Plan** — add, edit, delete, reorder or swap anything. Pick from a 160-exercise library or
invent your own with a name, machine and muscle. Edits apply to future sessions; your logged
history is never touched.

**Data** — export everything to a JSON file, import it back, or wipe the slate.

---

## Your data

Everything lives in your phone browser's `localStorage` under a single key, `gymTracker.v1`.

That means it is **private to this phone and this browser**. Nothing is uploaded anywhere.
It also means clearing Chrome's site data deletes it, so **export a backup now and then**
(Data → Export all data). If the browser ever blocks storage, the app keeps your session in
memory and tells you so at the top of the screen.

Weights are always stored in **kilograms**. The kg/lb switch only changes what is displayed,
so flipping it can never corrupt your history.

---

## Editing the plan in code

Open `index.html` and look for `SEED_PLAN` — it is the first thing in the script, a single
JSON object, and no UI code depends on its contents. Each exercise carries:

```js
{
  id: "mon-incline-chest-press-machine",
  name: "Incline Chest Press",
  machine: "Incline Chest Press Machine - plate loaded",
  primaryMuscle: "chest",
  secondaryMuscles: ["front delts", "triceps"],
  sets: 3,
  repRange: "8-12",
  restSeconds: 90,
  formCue: "Set the seat so the handles line up with your collarbones",
  type: "main"          // "warmup" | "main" | "cooldown"
}
```

After editing, open the app → **Plan** → **Reset plan to the original program**. That pulls in
your new `SEED_PLAN` and **keeps every logged session**.

Keep each `id` unique and stable — history is keyed on it. Change an exercise's `id` and it
starts a fresh history; change its `name` and the history follows along.

---

## Files

| File | What it is |
|---|---|
| `index.html` | The entire app — plan, library, UI, logic, styles |
| `sw.js` | Service worker, so it opens offline |
| `manifest.webmanifest` | Makes it installable as a home-screen app |
| `icon-*.png` | App icons |

`index.html` is genuinely self-contained: open it straight off disk and everything works
except the offline caching, which needs to be served over http(s).

---

## The program

Six training days, Push / Pull / Legs run twice a week, plus a Sunday mobility session.
Every muscle group is trained at least once a week and the big ones twice. Machines and
dumbbells carry most of the volume because they are safe to learn alone; a small number of
barbell lifts appear from week one so the skill is built early.

**Progression:** add reps inside the range first. Once you hit the top of the range on every
set, add the smallest jump the gym allows (usually 2.5 kg) and start again at the bottom of
the range. Leave 2–3 reps in reserve on every set — you should never be grinding.

Any day can be made a rest day for a single date, from the switch at the top of the Train
screen. That does not change the rest of your plan.
