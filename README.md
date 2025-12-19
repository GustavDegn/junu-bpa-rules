# powerbi-tabular-bpa-rules

Fælles **Best Practice Analyzer (BPA)**-regler til **Tabular Editor**.

Dette repo er vores *single source of truth* for BPA-rulesettet (`BPARules.json`). Scriptet herunder henter altid den nyeste version fra Azure DevOps og installerer den i din lokale Tabular Editor rules-mappe.

## Installer via Tabular Editor “Advanced Scripting”

Åbn **Tabular Editor → Advanced Scripting**, indsæt og kør:

```csharp
using System;
using System.IO;

string source = @"C:\temp\BPARules.json";

string localAppData = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
string teFolder = Path.Combine(localAppData, "TabularEditor3"); // TE3
Directory.CreateDirectory(teFolder);

string dest = Path.Combine(teFolder, "BPARules.json");
string backup = Path.Combine(teFolder, $"BPARules.backup.{DateTime.Now:yyyyMMdd_HHmmss}.json");

if (!File.Exists(source)) throw new FileNotFoundException("Source file not found: " + source);

// backup
if (File.Exists(dest)) File.Copy(dest, backup, true);

// copy
File.Copy(source, dest, true);

Info($"✅ BPA-regler kopieret til:\n{dest}\n\nÅbn BPA-vinduet og tjek 'Rules for the local user'. Genstart TE hvis de ikke dukker op med det samme.");

