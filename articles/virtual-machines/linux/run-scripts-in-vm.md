---
title: Köra skript i en Azure Linux-dator
description: Det här avsnittet beskriver hur du kör skript på en virtuell dator
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: cb811e3dba7be87c83b9893db682475351ada1c1
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267007"
---
# <a name="run-scripts-in-your-linux-vm"></a>Köra skript i Linux-VM

Du kan behöva köra kommandon på en virtuell dator för att automatisera uppgifter eller felsöka problem. I följande artikel ger en kort översikt över de funktioner som är tillgängliga att köra skript och -kommandon inom dina virtuella datorer.

## <a name="custom-script-extension"></a>Anpassat skripttillägg

Den [tillägget för anpassat skript](../extensions/custom-script-linux.md) används främst för efter distributionen konfigurations- och installationen.

* Hämta och köra skript i virtuella Azure-datorer.
* Du kan köra med hjälp av Azure Resource Manager-mallar, Azure CLI, REST-API, PowerShell eller Azure-portalen.
* Filer kan hämtas från Azure storage eller GitHub eller tillhandahålls från datorn när den körs från Azure-portalen.
* Kör PowerShell-skript i Windows-datorer och Bash-skript i Linux-datorer.
* Användbart för post distributionskonfiguration Programvaruinstallation, och andra konfiguration eller hanteringsuppgifter.

## <a name="run-command"></a>Kör kommando

Den [kommandot Kör](run-command.md) funktion gör det möjligt för virtuell dator och hantering och felsökning med hjälp av skript och är tillgängliga även när datorn kan inte nås, till exempel om gäst brandväggen inte har RDP eller SSH-port Öppna.

* Köra skript i virtuella Azure-datorer.
* Kan köras med [Azure-portalen](run-command.md), [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), eller [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand)
* Snabbt kör ett skript och visa utdata och upprepa efter behov i Azure-portalen.
* Skriptet kan skrivas direkt eller genom att köra något av de inbyggda skript.
* Kör PowerShell-skript i Windows-datorer och Bash-skript i Linux-datorer.
* Användbar för virtuell dator och hantering av program och för att köra skript i virtuella datorer som inte kan nås.

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker

Den [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) tillhandahåller allmänna dator, program och miljö med användarens anpassade skript som lagras i ett Automation-konto.

* Köra skript i Azure och Azure-datorer.
* Du kan köra med hjälp av Azure portal, Azure CLI, REST-API, PowerShell, webhooken.
* Skript lagras och hanteras i ett Automation-konto.
* Köra PowerShell, PowerShell-arbetsflöde, Python eller grafisk runbooks
* Ingen tidsgräns för skript för körning.
* Flera skript kan köras samtidigt.
* Utdata fullständig skriptet returnerade och lagras.
* Jobbhistorik är tillgängliga i 90 dagar.
* Skript kan köras som lokalt System eller med autentiseringsuppgifter som anges av användaren.
* Kräver [manuell installation](../../automation/automation-windows-hrw-install.md)

## <a name="serial-console"></a>Seriekonsol

Den [seriekonsolen](serial-console.md) ger direktåtkomst till en virtuell dator, liknar med ett tangentbord som är anslutna till den virtuella datorn.

* Kör kommandon på virtuella Azure-datorer.
* Du kan köra med hjälp av en textbaserad konsol till datorn i Azure-portalen.
* Logga in på datorn med ett lokalt användarkonto.
* Användbart när åtkomst till den virtuella datorn krävs oavsett tillståndet för den datorn nätverk eller operativsystem.

## <a name="next-steps"></a>Nästa steg

Mer information om de olika funktioner som är tillgängliga att köra skript och -kommandon inom dina virtuella datorer.

* [Anpassat skripttillägg](../extensions/custom-script-linux.md)
* [Kör kommando](run-command.md)
* [Runbook Worker-hybrid](../../automation/automation-hybrid-runbook-worker.md)
* [Seriekonsol](serial-console.md)