---
ms.date: 2017-06-05
keywords: powershell,cmdlet
title: Grundlegendes zur Windows PowerShell-Pipeline
ms.assetid: 6be50926-7943-4ef7-9499-4490d72a63fb
ms.openlocfilehash: 6d152e52d2fcfb9dd592eb9ac40500615f2186cb
ms.sourcegitcommit: 598b7835046577841aea2211d613bb8513271a8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2017
---
# <a name="understanding-the-windows-powershell-pipeline"></a><span data-ttu-id="d8f97-103">Grundlegendes zur Windows PowerShell-Pipeline</span><span class="sxs-lookup"><span data-stu-id="d8f97-103">Understanding the Windows PowerShell Pipeline</span></span>
<span data-ttu-id="d8f97-104">Die Weiterleitung über Pipes funktioniert nahezu überall in Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d8f97-104">Piping works virtually everywhere in Windows PowerShell.</span></span> <span data-ttu-id="d8f97-105">Obwohl Text auf dem Bildschirm angezeigt wird, leitet Windows PowerShell Text nicht zwischen Befehlen weiter.</span><span class="sxs-lookup"><span data-stu-id="d8f97-105">Although you see text on the screen, Windows PowerShell does not pipe text between commands.</span></span> <span data-ttu-id="d8f97-106">Stattdessen werden Objekte weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="d8f97-106">Instead, it pipes objects.</span></span>

<span data-ttu-id="d8f97-107">Die Notation für Pipelines ist ähnlich wie bei anderen Shells, weshalb es auf den ersten Blick möglicherweise nicht ersichtlich ist, dass in Windows PowerShell etwas Neues eingeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d8f97-107">The notation used for pipelines is similar to that used in other shells, so at first glance, it may not be apparent that Windows PowerShell introduces something new.</span></span> <span data-ttu-id="d8f97-108">Wenn Sie beispielsweise das Cmdlet **Out-Host** zum Erzwingen einer Seitenanzeige der Ausgabe eines anderen Befehls verwenden, sieht die Ausgabe bloß wie der normale auf dem Bildschirm gezeigte Text aus, der in Seiten unterteilt ist:</span><span class="sxs-lookup"><span data-stu-id="d8f97-108">For example, if you use the **Out-Host** cmdlet to force a page-by-page display of output from another command, the output looks just like the normal text displayed on the screen, broken up into pages:</span></span>

```
PS> Get-ChildItem -Path C:\WINDOWS\System32 | Out-Host -Paging

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\WINDOWS\system32

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2005-10-22  11:04 PM        315 $winnt$.inf
-a---        2004-08-04   8:00 AM      68608 access.cpl
-a---        2004-08-04   8:00 AM      64512 acctres.dll
-a---        2004-08-04   8:00 AM     183808 accwiz.exe
-a---        2004-08-04   8:00 AM      61952 acelpdec.ax
-a---        2004-08-04   8:00 AM     129536 acledit.dll
-a---        2004-08-04   8:00 AM     114688 aclui.dll
-a---        2004-08-04   8:00 AM     194048 activeds.dll
-a---        2004-08-04   8:00 AM     111104 activeds.tlb
-a---        2004-08-04   8:00 AM       4096 actmovie.exe
-a---        2004-08-04   8:00 AM     101888 actxprxy.dll
-a---        2003-02-21   6:50 PM     143150 admgmt.msc
-a---        2006-01-25   3:35 PM      53760 admparse.dll
<SPACE> next page; <CR> next line; Q quit
...
```

<span data-ttu-id="d8f97-109">Der Befehl „Out-Host -Paging“ ist ein nützliches Pipelineelement, wenn Sie eine lange Ausgabe haben, die langsam angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="d8f97-109">The Out-Host -Paging command is a useful pipeline element whenever you have lengthy output that you would like to display slowly.</span></span> <span data-ttu-id="d8f97-110">Er ist besonders nützlich, wenn der Vorgang sehr CPU-intensiv ist.</span><span class="sxs-lookup"><span data-stu-id="d8f97-110">It is especially useful if the operation is very CPU-intensive.</span></span> <span data-ttu-id="d8f97-111">Da die Verarbeitung an das Cmdlet „Out-Host“ übertragen wird, sobald eine vollständige Seite für die Anzeige bereit ist, wird der Vorgang von Cmdlets, die sich in der Pipeline davor befinden, angehalten, bis die nächste Seite der Ausgabe verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="d8f97-111">Because processing is transferred to the Out-Host cmdlet when it has a complete page ready to display, cmdlets that precede it in the pipeline halt operation until the next page of output is available.</span></span> <span data-ttu-id="d8f97-112">Sie können dies überprüfen, wenn Sie den Windows Task-Manager zum Überwachen der CPU- und Arbeitsspeicherauslastung durch Windows PowerShell verwenden.</span><span class="sxs-lookup"><span data-stu-id="d8f97-112">You can see this if you use the Windows Task Manager to monitor CPU and memory use by Windows PowerShell.</span></span>

<span data-ttu-id="d8f97-113">Führen Sie den folgenden Befehl aus: **Get-ChildItem C:\\Windows -Recurse**.</span><span class="sxs-lookup"><span data-stu-id="d8f97-113">Run the following command: **Get-ChildItem C:\\Windows -Recurse**.</span></span> <span data-ttu-id="d8f97-114">Vergleichen die CPU- und Arbeitsspeicherauslastung mit diesem Befehl: **Get-ChildItem C:\\Windows -Recurse | Out-Host -Paging**.</span><span class="sxs-lookup"><span data-stu-id="d8f97-114">Compare the CPU and memory usage to this command: **Get-ChildItem C:\\Windows -Recurse | Out-Host -Paging**.</span></span> <span data-ttu-id="d8f97-115">Was Sie auf dem Bildschirm sehen, ist Text, doch der Grund dafür ist, dass es erforderlich ist, Objekte im Konsolenfenster als Text darzustellen.</span><span class="sxs-lookup"><span data-stu-id="d8f97-115">What you see on the screen is text, but that is because it is necessary to represent objects as text in a console window.</span></span> <span data-ttu-id="d8f97-116">Dies ist lediglich eine Darstellung dessen, was innerhalb von Windows PowerShell wirklich passiert.</span><span class="sxs-lookup"><span data-stu-id="d8f97-116">This is just a representation of what is really going on inside Windows PowerShell.</span></span> <span data-ttu-id="d8f97-117">Nehmen Sie z.B. das Cmdlet „Get-Location“.</span><span class="sxs-lookup"><span data-stu-id="d8f97-117">For example, consider the Get-Location cmdlet.</span></span> <span data-ttu-id="d8f97-118">Bei Eingabe von **Get-Location**, während Ihr aktueller Speicherort der Stamm von Laufwerk C ist, sehen Sie die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="d8f97-118">If you type **Get-Location** while your current location is the root of the C drive, you would see the following output:</span></span>

```
PS> Get-Location

Path
----
C:\
```

<span data-ttu-id="d8f97-119">Wenn von Windows PowerShell Text weiterleitet wird, mit dem ein Befehl wie **Get-Location | Out-Host** aufgerufen wird, dann wird von **Get-Location** an **Out-Host** eine Gruppe von Zeichen entsprechend der Anzeige auf dem Bildschirm übergeben.</span><span class="sxs-lookup"><span data-stu-id="d8f97-119">If Windows PowerShell pipelined text, issuing a command such as **Get-Location | Out-Host**, would pass from **Get-Location** to **Out-Host** a set of characters in the order they are displayed onscreen.</span></span> <span data-ttu-id="d8f97-120">Wenn Sie also die Überschriftsinformationen ignorieren, empfängt **Out-Host** zuerst das Zeichen **C**, dann das Zeichen **:** und danach das Zeichen **\\**.</span><span class="sxs-lookup"><span data-stu-id="d8f97-120">In other words, if you were to ignore the heading information, **Out-Host** would first receive the character '**C'**, then the character '**:'**, then the character '**\\'**.</span></span> <span data-ttu-id="d8f97-121">Das Cmdlet **Out-Host** kann nicht die Bedeutung der Zeichen bestimmen, die vom Cmdlet **Get-Location** ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="d8f97-121">The **Out-Host** cmdlet could not determine what meaning to associate with the characters output by the **Get-Location** cmdlet.</span></span>

<span data-ttu-id="d8f97-122">Anstatt Befehle in einer Pipeline mithilfe von Text kommunizieren zu lassen, verwendet Windows PowerShell Objekte.</span><span class="sxs-lookup"><span data-stu-id="d8f97-122">Instead of using text to let commands in a pipeline communicate, Windows PowerShell uses objects.</span></span> <span data-ttu-id="d8f97-123">Aus Sicht eines Benutzers packen Objekte zusammengehörige Informationen in ein Format, welches das Bearbeiten der Informationen als Einheit und Extrahieren bestimmter Elemente vereinfacht, die Sie benötigen.</span><span class="sxs-lookup"><span data-stu-id="d8f97-123">From the standpoint of a user, objects package related information into a form that makes it easier to manipulate the information as a unit, and extract specific items that you need.</span></span>

<span data-ttu-id="d8f97-124">Der Befehl **Get-Location** gibt keinen Text zurück, der den aktuellen Pfad enthält.</span><span class="sxs-lookup"><span data-stu-id="d8f97-124">The **Get-Location** command does not return text that contains the current path.</span></span> <span data-ttu-id="d8f97-125">Er gibt ein Paket mit Informationen zurück, die als **PathInfo** -Objekt bezeichnet werden, das den aktuellen Pfad zusammen mit anderen Informationen enthält.</span><span class="sxs-lookup"><span data-stu-id="d8f97-125">It returns a package of information called a **PathInfo** object that contains the current path along with some other information.</span></span> <span data-ttu-id="d8f97-126">Das Cmdlet **Out-Host** gibt dann dieses **PathInfo**-Objekt auf dem Bildschirm aus. Windows PowerShell entscheidet anschließend, welche Informationen basierend auf den geltenden Formatierungsregeln wie angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="d8f97-126">The **Out-Host** cmdlet then sends this **PathInfo** object to the screen, and Windows PowerShell decides what information to display and how to display it based on its formatting rules.</span></span>

<span data-ttu-id="d8f97-127">Die vom Cmdlet **Get-Location** ausgegebenen Überschriftsinformationen werden erst am Ende des Vorgangs als Teil des Prozesses zur Formatierung der Daten für die Anzeige auf dem Bildschirm hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d8f97-127">In fact, the heading information output by the **Get-Location** cmdlet is added only at the end of the process, as part of the process of formatting the data for onscreen display.</span></span> <span data-ttu-id="d8f97-128">Auf dem Bildschirm sehen Sie eine Zusammenfassung der Informationen und keine vollständige Darstellung des Ausgabeobjekts.</span><span class="sxs-lookup"><span data-stu-id="d8f97-128">What you see onscreen is a summary of information, and not a complete representation of the output object.</span></span>

<span data-ttu-id="d8f97-129">Angenommen, ein Windows PowerShell-Befehl gibt mehr Informationen aus, als im Konsolenfenster angezeigt werden. Wie können wir die nicht sichtbaren Elemente abrufen?</span><span class="sxs-lookup"><span data-stu-id="d8f97-129">Given that there may be more information output from a Windows PowerShell command than what we see displayed in the console window, how can you retrieve the non-visible elements?</span></span> <span data-ttu-id="d8f97-130">Wie zeigen Sie die zusätzlichen Daten an?</span><span class="sxs-lookup"><span data-stu-id="d8f97-130">How do you view the extra data?</span></span> <span data-ttu-id="d8f97-131">Was ist, wenn Sie die Daten in einem anderen Format als in demjenigen anzeigen möchten, das Windows PowerShell normalerweise verwendet?</span><span class="sxs-lookup"><span data-stu-id="d8f97-131">And what if you want to view the data in a format different than the one Windows PowerShell normally uses?</span></span>

<span data-ttu-id="d8f97-132">Im weiteren Verlauf dieses Kapitels wird erörtert, wie Sie die Struktur bestimmter Windows PowerShell-Objekte erkennen, bestimmte Elemente auswählen und für eine einfachere Anzeige formatieren und diese Informationen an alternative Ausgabeziele wie Dateien oder Drucker übermitteln können.</span><span class="sxs-lookup"><span data-stu-id="d8f97-132">The rest of this chapter discusses how you can discover the structure of specific Windows PowerShell objects, selecting specific items and formatting them for easier display, and how to send this information to alternative output locations such as files and printers.</span></span>
