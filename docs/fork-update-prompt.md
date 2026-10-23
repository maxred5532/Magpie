# Updating this fork from upstream

Prompt for an agent. Paste it as-is, or point the agent at this file.

---

In this repo, update my fork from upstream (SAOG0721/Magpie, branch `experimental`) and keep my patch on top.

Remotes:

- `origin` is my fork (maxred5532/Magpie).
- `upstream` is the original repo (SAOG0721/Magpie).

Branches:

- `experimental` is a clean mirror of `upstream/experimental`. Never commit my own changes to it.
- `window-drag-handle` holds my patch. Read `docs/fork-patches.md` first. It lists every file and spot the patch touches.

Steps:

1. Make sure the working tree is clean. If it isn't, stop and ask me.
2. `git fetch upstream`
3. `git switch experimental`, then `git merge --ff-only upstream/experimental`. If the fast-forward fails, stop and tell me. Don't resolve it with a merge commit or reset.
4. `git push origin experimental`
5. `git switch window-drag-handle`, then `git rebase experimental`.
6. If there are conflicts, resolve them using `docs/fork-patches.md`:
   - Keep upstream's code and re-apply my small additions on top of it.
   - `FONTS_CACHE_VERSION` in `src/Magpie.Core/ImGuiFontsCacheManager.cpp` must stay higher than upstream's value. Use upstream's value plus one.
   - If upstream has fixed window dragging itself, or has changed the toolbar so my patch no longer makes sense, stop and tell me. Don't force it back in.
7. Update `docs/fork-patches.md` if any touched location moved.
8. Build to check it compiles. See "Building" below.
9. `git push --force-with-lease origin window-drag-handle`. Only do this if the rebase rewrote history and I've said pushing is fine.

Rules:

- Never push to `upstream`.
- Don't use pip. Use `uv` for Python tooling.
- Report what changed upstream (a short summary of the new commits) and anything you had to resolve by hand.

## Building

Needs Visual Studio Build Tools 2022 with the C++ and Universal Windows Platform build tools workloads, Windows SDK 26100, CMake, and Conan 2 (`uv tool install conan`, which puts it in `%USERPROFILE%\.local\bin`).

```powershell
$env:Path += ";$env:USERPROFILE\.local\bin"
& "C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\MSBuild\Current\Bin\amd64\MSBuild.exe" Magpie.slnx /t:Restore /p:Configuration=Release /p:Platform=x64 /p:RestorePackagesConfig=true
.\scripts\Build-Release.ps1 -AllowDirtySource -MaxCpuCount 4
```

The restore step downloads the NuGet packages. It's only needed the first time, or after package versions change.
