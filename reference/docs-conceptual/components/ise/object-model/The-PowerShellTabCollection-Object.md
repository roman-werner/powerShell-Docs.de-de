---
ms.date: 06/05/2017
keywords: powershell,cmdlet
title: Das PowerShellTabCollection-Objekt
ms.openlocfilehash: 0aad885afd3ba3ae3b00f5c11d2c62a9ff303798
ms.sourcegitcommit: 6545c60578f7745be015111052fd7769f8289296
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "75736112"
---
# <a name="the-powershelltabcollection-object"></a>Das PowerShellTabCollection-Objekt

Das **PowerShellTab**-Sammlungsobjekt ist eine Sammlung von **PowerShellTab**-Objekten. Jedes **PowerShellTab**-Objekt fungiert als eine separate Laufzeitumgebung. Dies ist eine Instanz der Microsoft.PowerShell.Host.ISE.PowerShellTabs-Klasse. Ein Beispiel ist das `$psISE.PowerShellTabs`-Objekt.

## <a name="methods"></a>Methoden

### <a name="add"></a>Add\(\)

In Windows PowerShell ISE 2.0 und höher unterstützt.

Fügt eine neue PowerShell-Registerkarte zur Sammlung hinzu. Die neu hinzugefügte Registerkarte wird zurückgegeben.

```powershell
$newTab = $psISE.PowerShellTabs.Add()
$newTab.DisplayName = 'Brand New Tab'
```

### <a name="removemicrosoftpowershellhostisepowershelltab-pstab"></a>Remove\(Microsoft.PowerShell.Host.ISE.PowerShellTab psTab\)

In Windows PowerShell ISE 2.0 und höher unterstützt.

Entfernt die Registerkarte, die durch den **psTab**-Parameter angegeben wird.

**psTab** Die zu entfernende PowerShell-Registerkarte.

```powershell
$newTab = $psISE.PowerShellTabs.Add()
Change the DisplayName of the new PowerShell tab.
$newTab.DisplayName = 'This tab will go away in 5 seconds'
sleep 5
$psISE.PowerShellTabs.Remove($newTab)
```

### <a name="setselectedpowershelltabmicrosoftpowershellhostisepowershelltab-pstab"></a>SetSelectedPowerShellTab\(Microsoft.PowerShell.Host.ISE.PowerShellTab psTab\)

In Windows PowerShell ISE 2.0 und höher unterstützt.

Wählt die PowerShell-Registerkarte aus, die durch den **psTab**-Parameter angegeben wird, um diese als aktuell aktive PowerShell-Registerkarte festzulegen.

**psTab** Die auszuwählende PowerShell-Registerkarte.

```powershell
# Save the current tab in a variable and rename it
$oldTab = $psISE.CurrentPowerShellTab
$psISE.CurrentPowerShellTab.DisplayName = 'Old Tab'
# Create a new tab and give it a new display name
$newTab = $psISE.PowerShellTabs.Add()
$newTab.DisplayName = 'Brand New Tab'
# Switch back to the original tab
$psISE.PowerShellTabs.SelectedPowerShellTab = $oldTab
```

## <a name="see-also"></a>Weitere Informationen

- [Das PowerShellTab-Objekt](The-PowerShellTab-Object.md)
- [Zweck des Windows PowerShell ISE-Skriptobjektmodells](Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [Die ISE-Objektmodellhierarchie](The-ISE-Object-Model-Hierarchy.md)
