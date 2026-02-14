# Shadeybane Client - GitHub Setup Guide

## Quick Setup Steps

### 1. Create GitHub Repository
1. Go to https://github.com/new
2. Repository name: `ShadeybaneClient` (or your preferred name)
3. Set to **Public**
4. **Don't** initialize with README (we have one)
5. Click "Create repository"

### 2. Initialize Git in Client Directory
```powershell
cd "c:\Program Files (x86)\Shadeybane"
git init
git add .
git commit -m "Initial commit - Shadeybane v1.0.0"
```

### 3. Push to GitHub
Replace `YOUR_USERNAME` with your GitHub username:
```powershell
git remote add origin https://github.com/YOUR_USERNAME/ShadeybaneClient.git
git branch -M main
git push -u origin main
```

### 4. Create First Release

**Option A: Via GitHub Web Interface**
1. Go to your repository on GitHub
2. Click "Releases" → "Create a new release"
3. Tag: `v1.0.0`
4. Title: `Shadeybane Client v1.0.0`
5. Description: Copy from `manifest.json` changelog
6. Upload the ZIP file (see step 5 below)
7. Click "Publish release"

**Option B: Via Command Line** (requires GitHub CLI)
```powershell
gh release create v1.0.0 Shadeybane-v1.0.0.zip --title "Shadeybane Client v1.0.0" --notes "Initial release"
```

### 5. Create Release ZIP

**PowerShell script to create ZIP:**
```powershell
$version = "1.0.0"
$source = "c:\Program Files (x86)\Shadeybane"
$destination = "$env:USERPROFILE\Desktop\Shadeybane-v$version.zip"

# Create ZIP excluding git files and patcher source
Compress-Archive -Path "$source\*" -DestinationPath $destination -Force

Write-Host "Created: $destination"
```

### 6. Calculate SHA-256 Hash
```powershell
$file = "$env:USERPROFILE\Desktop\Shadeybane-v1.0.0.zip"
$hash = (Get-FileHash $file -Algorithm SHA256).Hash
Write-Host "SHA-256: $hash"
```

Copy this hash and update `manifest.json` before pushing to GitHub.

### 7. Update Manifest
1. Edit `manifest.json`
2. Replace `YOUR_USERNAME` with your GitHub username
3. Replace `REPLACE_WITH_SHA256_HASH` with the hash from step 6
4. Commit and push:
```powershell
git add manifest.json
git commit -m "Update manifest with release info"
git push
```

### 8. Update Patcher Source Code
On your desktop where you moved the patcher source:
1. Edit `MainForm.cs` line 19
2. Replace `YOUR_USERNAME` with your GitHub username
3. Rebuild the patcher:
```powershell
dotnet publish -c Release -r win-x64 --self-contained true /p:PublishSingleFile=true
```
4. Copy the new `ShadeybaneUpe.exe` to your client folder

## Quick Commands Summary

```powershell
# 1. Create ZIP
Compress-Archive -Path "c:\Program Files (x86)\Shadeybane\*" -DestinationPath "$env:USERPROFILE\Desktop\Shadeybane-v1.0.0.zip" -Force

# 2. Get hash
(Get-FileHash "$env:USERPROFILE\Desktop\Shadeybane-v1.0.0.zip" -Algorithm SHA256).Hash

# 3. Initialize repo
cd "c:\Program Files (x86)\Shadeybane"
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/YOUR_USERNAME/ShadeybaneClient.git
git push -u origin main
```

## For Future Updates

1. Make changes to client files
2. Update version in `version.txt` and `manifest.json`
3. Create new ZIP with new version number
4. Calculate new SHA-256 hash
5. Create new GitHub release with new tag (e.g., `v1.0.1`)
6. Update `manifest.json` with new URL and hash
7. Push changes

The patcher will automatically detect and download the updates!
