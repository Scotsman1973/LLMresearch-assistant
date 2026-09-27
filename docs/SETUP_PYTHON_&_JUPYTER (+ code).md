# Setting up Jupyter Notebook locally (Windows, PowerShell)

This guide installs Python and Jupyter Notebook so you can run `localForager.ipynb` on your own computer. Every command is typed into **PowerShell**: press **Start**, type `PowerShell`, and open **Windows PowerShell**. No administrator rights are needed.

> **Note:** Python doesn't have long-term-support releases. Each version gets about five years of updates, so the safest choice is the **latest stable release**. As of September 2026 that is **Python 3.14**. The forager needs **3.10 or newer**, but 3.10 reaches end of life on 31 October 2026, so upgrading is recommended if that's what you have.

---

## 1. Check whether Python 3.10 or newer is already installed

Paste the whole block into PowerShell and press **Enter**:

```powershell
try {
    $out = & python --version 2>&1
    if ("$out" -match 'Python (\d+)\.(\d+)\.(\d+)') {
        $major = [int]$Matches[1]
        $minor = [int]$Matches[2]
        if ($major -gt 3 -or ($major -eq 3 -and $minor -ge 10)) {
            Write-Host "OK: $out is installed. Skip to step 3." -ForegroundColor Green
        } else {
            Write-Host "Found $out, which is too old. Continue to step 2." -ForegroundColor Yellow
        }
    } else {
        Write-Host "Python was not found. Continue to step 2." -ForegroundColor Yellow
    }
} catch {
    Write-Host "Python was not found. Continue to step 2." -ForegroundColor Yellow
}
```

If Windows opens the **Microsoft Store** instead, Python isn't installed. That's Windows' placeholder shortcut, not a real Python, so close the Store and continue to step 2.

---

## 2. Install the latest stable Python

This uses **winget**, which is built into Windows 10 and 11. It installs Python 3.14 for your user account only, adds it to your PATH, and includes the `py` launcher:

```powershell
winget install --id Python.Python.3.14 --exact --scope user --override "/quiet InstallAllUsers=0 PrependPath=1 Include_launcher=1"
```

When it finishes, **close PowerShell and open a new window** so the updated PATH takes effect. Then confirm it worked:

```powershell
python --version
```

It should print `Python 3.14.x`.

**If `winget` isn't recognised**, download the Windows installer from python.org/downloads instead. On the installer's first screen, tick **"Add python.exe to PATH"** before clicking **Install Now**.

---

## 3. Install Jupyter Notebook

Upgrade pip (Python's package installer) first, then install Notebook:

```powershell
python -m pip install --upgrade pip
python -m pip install notebook
```

Using `python -m pip` rather than just `pip` makes sure the package goes into the Python you just checked, even if you have more than one installed.

---

## 4. Move to the notebook's folder and start Jupyter

Replace the path with the folder that contains `localForager.ipynb`. Keep the quotation marks, since paths often contain spaces:

```powershell
Set-Location "$env:USERPROFILE\Desktop\research-assistant"
python -m notebook
```

Jupyter opens in your web browser. Click `localForager.ipynb` to open it, then choose **Run → Run All Cells**.

- **To stop Jupyter:** go back to PowerShell and press **Ctrl + C** (twice if asked).
- **Next time:** you only need step 4.

> **Why the folder matters:** the forager creates its temporary `research-workspace` folder in whichever folder Jupyter was started from. Starting it from the notebook's own folder keeps everything together. Google Drive for desktop should also be running before you start, so that `G:\My Drive\...` exists.
