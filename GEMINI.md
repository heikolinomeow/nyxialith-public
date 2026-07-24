## Employee context (HOS-92)

When prior work matters, do not assume a fresh, resumed, or peer-provider session still carries the relevant context. Use the employee-scoped `my_context` API with the injected agent token:

```sh
curl -sS "$HEIKOLES_API_URL/api/comms/my-context" \
  -H "Authorization: Bearer $HEIKOLES_API_TOKEN"
```

It returns only your recent conversation handles, your assigned compact ticket cards, and scheduled task state (when available). To look farther back, add `?older=true`. Choose a relevant handle, then read that conversation through the existing conversation API; do not treat the overview as a transcript or rely on an unrelated peer session's memory.

## Windows: `bash` means Git Bash (the bare name is a trap)

On this Windows machine the only `bash.exe` on PATH is `C:\WINDOWS\system32\bash.exe`, the **WSL** launcher — and WSL has no distro installed, so running bare `bash` returns the banner *"windows subsystem for linux has no installed distributions"* instead of a shell. The real, working bash ships with Git at `C:\Program Files\Git\bin\bash.exe`, but it is **not** on PATH under that name. Consequences: (1) run repo `.sh` scripts from a **Git Bash** terminal, not PowerShell/cmd; (2) scripts must never shell out to the bare word `bash` — resolve Git's bash by explicit path (e.g. `%ProgramFiles%\Git\bin\bash.exe`, or via `git --exec-path`), or accept an env-var override. Do **not** "fix" this by putting Git's `bin` ahead of `system32` on PATH — Git's `bin` also shadows Windows `find.exe`/`sort.exe` and breaks other tools. (Learned in heikolino-os 2026-07-23: a Python script's `subprocess.run(["bash", …])` hit WSL and every call failed with `unknown url type: windows subsystem for linux…`.)
