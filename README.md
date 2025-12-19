# powerbi-tabular-bpa-rules

Fælles **Best Practice Analyzer (BPA)**-regler til **Tabular Editor**.

Dette repo er vores *single source of truth* for BPA-rulesettet (`BPARules.json`).

## Installer via Tabular Editor “Advanced Scripting”

Åbn **Tabular Editor → Advanced Scripting**, indsæt og kør:

```csharp
using System;
using System.IO;
using System.Net;

ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;

string url = "https://raw.githubusercontent.com/GustavDegn/junu-bpa-rules/main/BPARules.json";

string localAppData = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
string teFolder = Path.Combine(localAppData, "TabularEditor3");
Directory.CreateDirectory(teFolder);

string dest = Path.Combine(teFolder, "BPARules.json");
string backup = Path.Combine(teFolder, $"BPARules.backup.{DateTime.Now:yyyyMMdd_HHmmss}.json");

if (File.Exists(dest)) File.Copy(dest, backup, true);

using (var w = new WebClient())
{
    w.Headers.Add(HttpRequestHeader.Accept, "application/json");
    w.DownloadFile(url, dest);
}

var text = File.ReadAllText(dest).TrimStart();
if (!text.StartsWith("["))
    throw new Exception("Downloaded file does not look like JSON (maybe an error page).");

Info($"✅ BPA-regler installeret til:\n{dest}\n\nGenåbn BPA-vinduet eller genstart Tabular Editor hvis de ikke dukker op med det samme.");

