# Live composer-reader drive — claude 2.1.285, `claude -n fm-lab-probe` (#5558)

Real named Claude Code session launched in a disposable lab on a private tmux
socket, sized 200x50. The idle pane rendered the exact reported geometry:

    ────────────────────────── fm-lab-probe ─   (titled top rule)
    ❯                                            (bare agent-glyph row)
    ──────────────────────────────────────────  (solid bottom rule)

The real classifier `fm_composer_classify_screen` (bin/fm-composer-lib.sh) was
run on the captured LIVE bytes.

## Idle composer
| profile              | BASE (pre-fix)        | FIX               |
|----------------------|-----------------------|-------------------|
| herdr (cursor=0)     | unknown  <-- the bug  | empty             |
| tmux  (cursor=1)     | empty                 | empty             |

Under the herdr capability profile the pre-fix code read the idle titled
composer as `unknown` — reproducing #5558 (fm-send doorbells failed,
fm-control exit/relaunch refused). The fix reads it `empty`.

## Typed draft (unsubmitted)
Typed a real draft into the live composer without Enter.
- FIX (herdr and tmux profiles): `pending`
- FIX extraction: the exact typed text, footer excluded.
- BASE (herdr profile): `unknown`.

## Key-only exit mechanic (fm_control_unreadable_exit_keys claude = "C-c 2")
On the live idle pane holding a draft:
- Escape (interrupt) then first Ctrl+C  -> composer CLEARED (draft discarded,
  never submitted), "Press Ctrl-C again to exit" armed.
- Second Ctrl+C -> claude exited to the shell.
This confirms the documented empirical basis: first C-c discards the composer,
second C-c exits. A draft the reader could not see is discarded, never sent.
