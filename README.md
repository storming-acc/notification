
---

# 📚 NoyaUI Notification System — Complete Documentation

**Version:** Ultimate Edition (Auto-Scale Update)  
**Author:** NoyaUI Team  
**License:** GNU General Public License   
**Compatibility:** Any Roblox executor with Luau support  

---

## 🎯 1. Introduction

NoyaUI is a fully‑featured, standalone notification system for Roblox scripts. It brings professional, interactive pop‑ups to any game or tool – from simple toasts to complex dialogs with buttons, progress bars, images, and even 3D previews.

**Why choose NoyaUI?**
* **📏 Smart Auto-Scaling [NEW]** – No more cut-off text! The UI perfectly calculates the required height based on your description, buttons, and progress bars.
* **📐 Strict Size Overrides [NEW]** – Need a specific dimension? Pass `Width` and `Height` to force exact pixel sizes.
* **🧩 Plug‑and‑play** – One‑line installation, no external dependencies.
* **📦 Self‑contained** – Creates its own ScreenGui and manages the UI lifecycle.
* **🧠 Smart Queuing** – Auto‑queues notifications when a corner is full (max 5) and prevents duplicates.
* **⚡ Responsive** – Smooth tween animations and dynamic per‑position stacking.
* **🖼️ Rich Media** – Images from Roblox assets, direct URLs, Google Drive, and decal IDs (auto‑converted).
* **🔘 Interactive** – Dual buttons with custom colors, tooltips, icons, and callbacks.
* **🔄 Live Updates** – Change title, description, progress, or position on the fly.

---

## 🚀 2. Installation

### ✅ Primary Method (Recommended)
Load the script from your hosted source using `loadstring`:
```lua
local NoyaUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/storming-acc/notification/refs/heads/main/NoyaUI_library"))()
```

### 📦 Alternative Method – Paste the Full Code
Copy the entire NoyaUI script and paste it at the top of your main script.

### 📁 Option B – Use as a ModuleScript
Place a ModuleScript inside `ReplicatedStorage`, name it `NoyaUI`, paste the code, and require it:
```lua
local NoyaUI = require(game.ReplicatedStorage.NoyaUI)
```

---

## ⚡ 3. Quick Start – Your First Notification

```lua
NoyaUI:Notify({
    Title = "Hello World",
    Desc = "This is a simple notification."
})
```
This shows a blue "Info" notification in the bottom‑right corner lasting 5 seconds. The height will automatically adjust to fit the text perfectly.

---

## 📋 4. Core Concepts

| Concept | Description |
| :--- | :--- |
| **Position** | Five corners: `"TopRight"`, `"TopLeft"`, `"BottomRight"`, `"BottomLeft"`, `"Center"`. Default is `"BottomRight"`. |
| **Type** | Four visual styles: `"Info"` (blue), `"Success"` (green), `"Warn"` (yellow), `"Error"` (red). |
| **Duration** | Seconds before auto‑dismiss. `0` makes the notification sticky (stays until closed manually). |
| **Auto-Size** | The notification perfectly stretches its Y-Axis based on the length of your `Desc`. |
| **Queue** | If a position already has 5 notifications, new ones queue up silently in the background. |

---

## 🔧 5. Full Parameter Reference

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **Title** | `string` | `"Notification"` | Main bold text. Supports RichText if enabled. |
| **Desc** | `string` | `""` | Secondary text. **The UI height automatically scales based on this length.** |
| **Duration** | `number` | `5` | Seconds before auto‑close. `0` = sticky. |
| **Type** | `string` | `"Info"` | `"Info"`, `"Success"`, `"Warn"`, `"Error"`. |
| **Position** | `string` | `"BottomRight"`| The screen corner to spawn in. |
| **AutoSize** | `boolean` | `true` | When true, the UI height dynamically fits the text length. |
| **Width** | `number` | `200` | Custom width in pixels. |
| **Height** | `number` | `nil` | **[NEW]** Forces an exact pixel height, ignoring AutoSize. |
| **Image** | `string/number`| `nil` | Asset ID, decal ID, direct URL, or Google Drive link. |
| **CornerRadius**| `number` | `8` | Rounds the corners of the notification. |
| **RichText** | `boolean` | `false` | Enables HTML tags like `<b>`, `<i>`, `<font color='#FFF'>`. |
| **Progress** | `number` | `nil` | `0` to `100`. Shows a horizontal progress bar. |
| **Buttons** | `table` | `[]` | Up to 2 buttons: `{ Text, Callback, ButtonColor, TextColor, Icon, Tooltip }`. |
| **OnClose** | `function` | `nil` | Fires just before the notification is destroyed. |
| **ID** | `string` | `auto` | Unique identifier for updating/moving later. |

---

## 📏 6. Detailed Features

### 📐 The Auto-Scaling Engine (New!)
NoyaUI now uses Roblox's `TextService` to read exactly how many pixels your `Desc` text takes up. 
* You can write a massive paragraph, and the notification will instantly stretch down to fit it flawlessly.
* It calculates the height of the text *plus* the height of any Buttons, Images, or Progress Bars you have active, ensuring no elements ever overlap.

### 🖼️ Images
Attach images in three ways:
* **Asset ID:** `Image = 123456789`
* **Direct URL:** `Image = "https://example.com/image.png"` (Cached in workspace)
* **Google Drive:** `Image = "https://drive.google.com/file/d/.../view"`

### 🔘 Buttons
Add up to two interactive buttons. 
If the callback executes successfully, the notification will automatically close. You can prevent this by having the callback return `false`.
```lua
Buttons = {
    { 
        Text = "Confirm", 
        ButtonColor = Color3.fromRGB(145, 255, 128),
        Callback = function() print("Clicked!") end 
    }
}
```

### 🔄 Live Updating
Update a notification's text, progress bar, or duration while it's still on screen!
```lua
local notif = NoyaUI:Notify({ Title = "Loading", Duration = 0, Progress = 0 })
task.wait(1)
NoyaUI:Update(notif.id, { Progress = 50, Desc = "Halfway there!" })
```

---

## 💡 7. Complete Examples

### 1. The Auto-Scaling Text Showcase
```lua
NoyaUI:Notify({
    Title = "Changelog v2.0",
    Desc = "This is a massive paragraph to test the new Automatic Text Scaling feature! If this is working correctly, the notification will stretch vertically to fit all of this text without any overlapping. It calculates the Y-axis boundary instantly before tweening!",
    Duration = 10,
    Type = "Success"
})
```

### 2. Exact Override Size (No Auto-Scaling)
```lua
NoyaUI:Notify({
    Title = "Fixed Size Menu",
    Desc = "This ignores auto-scaling because we hardcoded Width and Height.",
    Duration = 0,
    Width = 350,
    Height = 150
})
```

### 3. Download with Progress & Buttons
```lua
local dl = NoyaUI:Notify({
    Title = "Downloading...",
    Progress = 10,
    Duration = 0,
    Buttons = {
        { Text = "Cancel", ButtonColor = Color3.fromRGB(255, 82, 82) }
    }
})

for i = 10, 100, 10 do
    task.wait(0.5)
    NoyaUI:Update(dl.id, { Progress = i, Desc = i .. "% completed" })
end
```

---

## ❓ 8. Troubleshooting

| Issue | Solution |
| :--- | :--- |
| **Text is cut off** | Ensure `AutoSize` is not set to `false`, and make sure you aren't passing a hardcoded `Height` property in your config. |
| **Notification doesn't appear** | Check if the position already has 5 notifications. It is queued and will appear as soon as one closes. |
| **Image not showing** | Ensure the executor supports `game:HttpGet` and `getcustomasset`. Check the F9 console for download warnings. |
| **Tooltips not working** | Ensure the executor supports `RunService.RenderStepped`. |

---

**Documentation updated:** August 2026 – NoyaUI Ultimate Edition
