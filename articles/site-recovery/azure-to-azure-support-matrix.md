---
title: Azure Site Recovery-Supportmatrix zum Replizieren aus Azure in Azure | Microsoft-Dokumentation
description: Übersicht über die unterstützten Betriebssysteme und Konfigurationen für die Azure Site Recovery-Replikation von virtuellen Azure-Computern (VMs) aus einer Region in eine andere für die Notfallwiederherstellung.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: sujayt
ms.openlocfilehash: 49773e076ed8bb06ff76f9f654b914a709051fb5
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378617"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Unterstützungsmatrix für die Replikation von einer Azure-Region in eine andere



In diesem Artikel werden die unterstützten Konfigurationen und Komponenten bei der Replikation und Wiederherstellung von virtuellen Azure-Computern zwischen verschiedenen Regionen mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst beschrieben.

## <a name="user-interface-options"></a>Optionen der Benutzeroberfläche

**Benutzeroberfläche** |  **Unterstützt/Nicht unterstützt**
--- | ---
**Azure-Portal** | Unterstützt
**PowerShell** | [Replikation von Azure zu Azure mit PowerShell](azure-to-azure-powershell.md)
**REST-API** | Derzeit nicht unterstützt
**BEFEHLSZEILENSCHNITTSTELLE (CLI)** | Derzeit nicht unterstützt


## <a name="resource-support"></a>Ressourcenunterstützung

**Typ des Verschiebevorgangs für Ressourcen** | **Details**
--- | --- | ---
**Tresor über Ressourcengruppen hinweg verschieben** | Nicht unterstützt<br/><br/> Sie können den Recovery Services-Tresor nicht über Ressourcengruppen hinweg verschieben.
**Verschieben von Compute-, Speicher- und Netzwerkressourcen über Ressourcengruppen hinweg** | Nicht unterstützt.<br/><br/> Wenn Sie eine VM oder zugehörige Komponenten wie Speicher bzw. Netzwerke verschieben, nachdem diese repliziert wurden, müssen Sie die Replikation deaktivieren und die Replikation für die VM erneut aktivieren.
**Replizieren von Azure-VMs von einem Abonnement in ein anderes zur Notfallwiederherstellung** | Innerhalb des gleichen Azure Active Directory-Mandanten unterstützt. Für klassische VMs nicht unterstützt.
**Migrieren von VMs zwischen Regionen innerhalb der unterstützten geografischen Cluster (innerhalb und über Abonnements hinweg)** | Im gleichen Azure Active Directory-Mandanten für VMs mit dem Resource Manager-Bereitstellungsmodell unterstützt. Für VMs mit dem klassischen Bereitstellungsmodell nicht unterstützt.
**Migrieren von VMs in derselben Region** | Nicht unterstützt.


## <a name="support-for-replicated-machine-os-versions"></a>Unterstützung für replizierte Computer-Betriebssystemversionen

Die unten aufgeführte Unterstützung gilt für alle Workloads unter dem genannten Betriebssystem.

#### <a name="windows"></a>Windows

- Windows Server 2016 (Server Core, Server mit Desktopdarstellung)*
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 mit mindestens SP1

>[!NOTE]
>
> \*Windows Server 2016, Nano Server wird nicht unterstützt.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5   
- CentOS 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3,7.4, 7.5
- Ubuntu 14.04 LTS Server[ (unterstützte Kernel-Versionen)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server[ (unterstützte Kernel-Versionen)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Debian 7 [ (unterstützte Kernel-Versionen)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- Debian 8 [ (unterstützte Kernel-Versionen)](#supported-debian-kernel-versions-for-azure-virtual-machines)
- SUSE Linux Enterprise Server 12 SP1, SP2, SP3 [ (unterstützte Kernelversionen)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
- SUSE Linux Enterprise Server 11 SP3
- SUSE Linux Enterprise Server 11 SP4
- Oracle Linux 6.4, 6.5, 6.6, 6.7, auf dem entweder der Red Hat-kompatible Kernel oder UEK3 (Unbreakable Enterprise Kernel Release 3) ausgeführt wird

(Ein Upgrade von replizierenden Computern von SLES 11 SP3 auf SLES 11 SP4 wird nicht unterstützt. Wenn für einen replizierten Computer ein Upgrade von SLES 11 SP3 auf SLES 11 SP4 durchgeführt wurde, müssen Sie die Replikation deaktivieren und den Computer nach dem Upgrade erneut schützen.)

>[!NOTE]
>
> Bei Ubuntu-Servern mit kennwortbasierter Authentifizierung und Anmeldung, auf denen das Paket cloud-init zum Konfigurieren von Cloud-VMs verwendet wird, kann bei einem Failover (abhängig von der cloud-init-Konfiguration) die kennwortbasierte Anmeldung deaktiviert sein. Die kennwortbasierte Anmeldung kann auf der VM wieder aktiviert werden, indem das Kennwort im Azure-Portal im Menü „Einstellungen“ (im Abschnitt „SUPPORT + PROBLEMBEHANDLUNG“) der VM nach erfolgtem Failover zurückgesetzt wird.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Unterstützte Ubuntu-Kernelversionen für virtuelle Azure-Computer

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
14.04 LTS | 9.19 | 3.13.0-24-generic bis 3.13.0-153-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-131-generic, |
14.04 LTS | 9.18 | 3.13.0-24-generic bis 3.13.0-151-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-128-generic |
14.04 LTS | 9.17 | 3.13.0-24-generic bis 3.13.0-147-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-124-generic |
14.04 LTS | 9.16 | 3.13.0-24-generic bis 3.13.0-144-generic,<br/>3.16.0-25-generic bis 3.16.0-77-generic,<br/>3.19.0-18-generic bis 3.19.0-80-generic,<br/>4.2.0-18-generic bis 4.2.0-42-generic,<br/>4.4.0-21-generic bis 4.4.0-119-generic |
|||
16.04 LTS | 9.19 | 4.4.0-21-generic bis 4.4.0-131-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.15.0-13-generic bis 4.15.0-30-generic<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure <br/>4.15.0-1012-azure bis 4.15.0-1019-azure|
16.04 LTS | 9.18 | 4.4.0-21-generic bis 4.4.0-128-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-45-generic,<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1018-azure |
16.04 LTS | 9.17 | 4.4.0-21-generic bis 4.4.0-124-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-41-generic,<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1016-azure |
16.04 LTS | 9.16 | 4.4.0-21-generic bis 4.4.0-119-generic,<br/>4.8.0-34-generic bis 4.8.0-58-generic,<br/>4.10.0-14-generic bis 4.10.0-42-generic,<br/>4.11.0-13-generic bis 4.11.0-14-generic,<br/>4.13.0-16-generic bis 4.13.0-38-generic,<br/>4.11.0-1009-azure bis 4.11.0-1016-azure,<br/>4.13.0-1005-azure bis 4.13.0-1012-azure |


### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Unterstützte Debian-Kernelversionen für virtuelle Azure-Computer

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
Debian 7 | 9.17, 9.18, 9.19 | 3.2.0-4-amd64 bis 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.16 | 3.2.0-4-amd64 bis 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.19 | 3.16.0-4-amd64 bis 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.7-amd64 |
Debian 8 | 9.17, 9.18 | 3.16.0-4-amd64 bis 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.16 | 3.16.0-4-amd64 bis 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 bis 4.9.0-0.bpo.5-amd64 |

### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Unterstützte SUSE Linux Enterprise Server 12-Kernelversionen für Azure-VMs

**Release** | **Mobility Service-Version** | **Kernelversion** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.19 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default bis 4.4.140-94.42-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.18 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.93-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default bis 4.4.121-92.80-default</br></br>SP3 4.4.73-5-default bis 4.4.138-94.39-default |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3) | 9.17 | SP1 3.12.49-11-default bis 3.12.74-60.64.40-default</br></br> SP1(LTSS) 3.12.74-60.64.45-default bis 3.12.74-60.64.88-default</br></br> SP2 4.4.21-69-default bis 4.4.120-92.70-default</br></br>SP2(LTSS) 4.4.121-92.73-default</br></br>SP3 4.4.73-5-default bis 4.4.126-94.22-default |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Unterstützte Dateisysteme und Gastspeicherkonfigurationen auf virtuellen Azure-Computern unter Linux

* Dateisysteme: ext3, ext4, ReiserFS (nur Suse Linux Enterprise Server), XFS
* Volume-Manager: LVM2
* Multipfadsoftware: Gerätemapper

## <a name="region-support"></a>Unterstützung für Regionen

Sie können virtuelle Computer zwischen zwei beliebigen Regionen im gleichen geografischen Cluster replizieren.

**Geografischer Cluster** | **Azure-Regionen**
-- | --
Amerika | Kanada, Osten; Kanada, Mitte; USA, Süden-Mitte; USA, Westen-Mitte; USA, Osten; USA, Osten 2; USA, Westen; USA, Westen 2; USA, Mitte; USA, Norden-Mitte
Europa | „Vereinigtes Königreich, Westen“, „Vereinigtes Königreich, Süden“, „Europa, Norden“, „Europa, Westen“, „Frankreich, Mitte“, „Frankreich, Süden“
Asien | Indien, Süden; Indien, Mitte; Asien, Südosten; Asien, Osten; Japan, Osten; Japan, Westen; Korea, Mitte; Korea, Süden
Australien   | Australien, Osten; Australien, Südosten; Australien, Mitte; Australien, Mitte 2
Azure Government    | „US GOV Virginia“; „US GOV Iowa“; „US GOV Arizona“; „US GOV Texas“; „US DoD, Osten“; „US DoD, Mitte“
Deutschland | „Deutschland, Mitte“; „Deutschland, Nordosten“
China | „China, Osten“, „China, Norden“

>[!NOTE]
>
> Für die Region „Brasilien, Süden“ sind Replikation, Failover und Failback nur zu „USA, Süden-Mitte“, „USA, Westen-Mitte“, „USA, Osten“, „USA, Osten 2“, „USA, Westen“, „USA, Westen 2“ und „USA, Norden-Mitte“ möglich.

## <a name="support-for-vmdisk-management"></a>Unterstützung für die VM-/Datenträgerverwaltung

**Aktion** | **Details**
-- | ---
Größe des Datenträgers auf einer replizierten VM ändern | Unterstützt
Datenträger zu einer replizierten VM hinzufügen | Nicht unterstützt. Sie müssen die Replikation für die VM deaktivieren, den Datenträger hinzufügen und die Replikation dann erneut aktivieren.


## <a name="support-for-compute-configuration"></a>Unterstützung für Computekonfiguration

**Konfiguration** | **Unterstützt/Nicht unterstützt.** | **Anmerkungen**
--- | --- | ---
Größe | Jede Größe von virtuellen Azure-Computern mit mindestens 2 CPU-Kernen und 1 GB RAM | Weitere Informationen finden Sie unter [Größen für virtuelle Azure-Computer](../virtual-machines/windows/sizes.md).
Verfügbarkeitsgruppen | Unterstützt | Wenn Sie die Standardoption beim Schritt „Replikation aktivieren“ im Verwaltungsportal verwenden, wird die Verfügbarkeitsgruppe automatisch basierend auf der Konfiguration der Quellregion erstellt. Sie können die Zielverfügbarkeitsgruppe jederzeit unter „Repliziertes Element“ > „Einstellungen“ > „Compute und Netzwerk“ > „Verfügbarkeitsgruppe“ ändern.
Verfügbarkeitszonen | Nicht unterstützt | VMs, die in Verfügbarkeitszonen bereitgestellt werden, werden derzeit nicht unterstützt.
Virtuelle Computer mit Hybridnutzungsvorteil (HUB) | Unterstützt | Wenn für den virtuellen Quellcomputer eine HUB-Lizenz aktiviert wurde, verwendet auch der virtuelle Computer für Testfailover oder Failover die HUB-Lizenz.
VM-Skalierungsgruppen | Nicht unterstützt |
Azure-Katalogimages – von Microsoft veröffentlicht | Unterstützt | Unterstützt, solange der virtuelle Computer unter einem von Site Recovery unterstützten Betriebssystem ausgeführt wird.
Azure-Katalogimages – von Drittanbietern veröffentlicht | Unterstützt | Unterstützt, solange der virtuelle Computer unter einem von Site Recovery unterstützten Betriebssystem ausgeführt wird.
Benutzerdefinierte Images – von Drittanbietern veröffentlicht | Unterstützt | Unterstützt, solange der virtuelle Computer unter einem von Site Recovery unterstützten Betriebssystem ausgeführt wird.
Mit Site Recovery migrierte virtuelle Computer | Unterstützt | Wenn ein VMware-/physischer Computer mit Site Recovery in Azure migriert wurde, müssen Sie die ältere Version des Mobility Service deinstallieren und den Computer neu starten, bevor er in einer anderen Azure-Region repliziert werden kann.

## <a name="support-for-storage-configuration"></a>Unterstützung für Speicherkonfiguration

**Konfiguration** | **Unterstützt/Nicht unterstützt.** | **Anmerkungen**
--- | --- | ---
Maximale Datenträgergröße für das Betriebssystem | 2.048 GB | Weitere Informationen finden Sie unter [Von VMs verwendete Datenträger](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Maximale Größe des Datenträgers für Daten | 4095 GB | Weitere Informationen finden Sie unter [Von VMs verwendete Datenträger](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Anzahl von Datenträgern für Daten | Bis zu 64, sofern von einer bestimmten Größe für virtuelle Azure-Computer unterstützt | Weitere Informationen finden Sie unter [Größen für virtuelle Azure-Computer](../virtual-machines/windows/sizes.md).
Temporärer Datenträger | Immer von der Replikation ausgeschlossen | Temporäre Datenträger sind immer von der Replikation ausgeschlossen. Gemäß den Azure-Richtlinien sollten Sie keine persistenten Daten auf einem temporären Datenträger speichern. Unter [Temporärer Datenträger für virtuelle Azure-Computer](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) finden Sie weitere Informationen.
Datenänderungsrate auf dem Datenträger | Maximal 10 MBit/s pro Datenträger bei Storage Premium und 2 MBit/s pro Datenträger bei Storage Standard | Wenn die durchschnittliche Datenänderungsrate auf dem Datenträger dauerhaft über 10 MBit/s (bei Premium) und 2 MBit/s (bei Standard) liegt, kann die Replikation nicht folgen. Wenn nur gelegentlich eine große Datenmenge anfällt und die Datenänderungsrate zeitweise über 10 MBit/s (bei Premium) und 2 MBit/s (bei Standard) liegt und dann zurückgeht, kann die Replikation folgen. In diesem Fall kann es möglicherweise zu etwas verzögerten Wiederherstellungspunkten kommen.
Datenträger in Standardspeicherkonten | Unterstützt |
Datenträger in Storage Premium-Konten | Unterstützt | Wenn ein virtueller Computer Datenträger in Premium- und Standard-Speicherkonten aufweist, können Sie für jeden Datenträger ein eigenes Zielspeicherkonto auswählen, um sicherzustellen, dass die gleiche Speicherkonfiguration in der Zielregion vorhanden ist.
Verwaltete Standard-Datenträger | Unterstützt in Azure-Regionen, in denen Azure Site Recovery unterstützt wird. |  
Verwaltete Premium-Datenträger | Unterstützt in Azure-Regionen, in denen Azure Site Recovery unterstützt wird. |
Speicherplätze | Unterstützt |         
Verschlüsselung ruhender Daten (SSE) | Unterstützt | SSE ist die Standardeinstellung für Speicherkonten.   
Azure Disk Encryption (ADE) für Windows | VMs, die für die [Verschlüsselung mit der Azure AD-App aktiviert sind](https://aka.ms/ade-aad-app), werden unterstützt. |
Azure Disk Encryption (ADE) für Linux | Nicht unterstützt |
Datenträger laufendem Systembetrieb hinzufügen/entfernen | Nicht unterstützt | Wenn Sie Datenträger auf dem virtuellen Computer hinzufügen oder entfernen, müssen Sie die Replikation deaktivieren und dann für den virtuellen Computer wieder aktivieren.
Ausschließen von Datenträgern | Nicht unterstützt|   Temporäre Datenträger sind standardmäßig ausgeschlossen.
Speicherplätze direkt  | Nicht unterstützt|
Dateiserver mit horizontaler Skalierung  | Nicht unterstützt|
LRS | Unterstützt |
GRS | Unterstützt |
RA-GRS | Unterstützt |
ZRS | Nicht unterstützt |  
Kalter und heißer Speicher | Nicht unterstützt | Datenträger für virtuelle Computer werden auf kaltem und heißem Speicher nicht unterstützt.
Azure Storage-Firewalls für virtuelle Netzwerke  | Nein  | Das Gewähren des Zugriffs auf bestimmte virtuelle Azure-Netzwerke auf Cachespeicherkonten, die zum Speichern replizierter Daten verwendet werden, wird nicht unterstützt.
Allgemeine V2-Speicherkonten (heiße und kalte Ebene) | Nein  | Die Transaktionskosten steigen gegenüber allgemeinen V1-Speicherkonten erheblich an.

>[!IMPORTANT]
> Stellen Sie sicher, dass Sie die Skalierbarkeits- und Leistungsziele für VM-Datenträger für virtuelle [Linux](../virtual-machines/linux/disk-scalability-targets.md)- oder [Windows](../virtual-machines/windows/disk-scalability-targets.md)-Computer beachten, um Leistungsprobleme zu vermeiden. Wenn Sie die Standardeinstellungen übernehmen, erstellt Site Recovery die erforderlichen Datenträger und Speicherkonten auf Basis der Quellkonfiguration. Wenn Sie Ihre eigenen Einstellungen anpassen und verwenden möchten, achten Sie darauf, die Skalierbarkeits- und Leistungsziele für Datenträger für Ihre virtuellen Quellcomputer einzuhalten.

## <a name="support-for-network-configuration"></a>Unterstützung der Netzwerkkonfiguration
**Konfiguration** | **Unterstützt/Nicht unterstützt.** | **Anmerkungen**
--- | --- | ---
Netzwerkschnittstelle (NIC) | Bis zur maximalen Anzahl von NICs, die von einer bestimmten Größe von virtuellen Azure-Computern unterstützt werden | Netzwerkkarten werden bei der Erstellung des virtuellen Computers als Teil des Testfailover- oder Failovervorgangs erstellt. Die Anzahl der Netzwerkkarten auf dem virtuellen Failovercomputer ist abhängig von der Anzahl der Netzwerkkarten, die auf dem virtuellen Quellcomputer vorhanden waren, als die Replikation aktiviert wurde. Wenn Sie Netzwerkkarten nach dem Aktivieren der Replikation hinzufügen/entfernen, hat dies keine Auswirkungen auf den virtuellen Failovercomputer.
Internetlastenausgleich | Unterstützt | Sie müssen den vorkonfigurierten Lastenausgleich mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan zuordnen.
Interner Lastenausgleich | Unterstützt | Sie müssen den vorkonfigurierten Lastenausgleich mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan zuordnen.
Öffentliche IP-Adresse| Unterstützt | Sie müssen eine bereits vorhandene öffentliche IP-Adresse der Netzwerkkarte zuordnen oder eine IP-Adresse erstellen und der Netzwerkkarte mit einem Azure Automatisierungsskript in einem Wiederherstellungsplan zuordnen.
NSG auf Netzwerkkarte (Ressourcen-Manager)| Unterstützt | Sie müssen die NSG mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan der Netzwerkkarte zuordnen.  
NSG in Subnetz (Ressourcen-Manager und klassische Bereitstellung)| Unterstützt | Sie müssen die NSG mithilfe eines Azure-Automatisierungsskript dem Subnetz in einem Wiederherstellungsplan zuordnen.
NSG auf virtuellem Computer (klassische Bereitstellung)| Unterstützt | Sie müssen die NSG mit einem Azure-Automatisierungsskript in einem Wiederherstellungsplan der Netzwerkkarte zuordnen.
Reservierte IP (statische IP-Adresse)/Quell-IP beibehalten | Unterstützt | Wenn die NIC auf dem virtuellen Quellcomputer eine statische IP-Konfiguration aufweist und im Zielsubnetz die gleiche IP-Adresse verfügbar ist, wird sie dem virtuellen Failovercomputer zugewiesen. Wenn im Zielsubnetz nicht die gleiche IP-Adresse verfügbar ist, wird eine der verfügbaren IP-Adressen im Subnetz für diesen virtuellen Computer reserviert. Sie können unter „Repliziertes Element“ > „Einstellungen“ > „Compute und Netzwerk“ > „Netzwerkschnittstellen“ eine beliebige feste IP-Adresse angeben. Sie können die Netzwerkkarte auswählen und das gewünschte Subnetz und die IP-Adresse angeben.
Dynamische IP| Unterstützt | Wenn die Netzwerkkarte auf dem virtuellen Quellcomputer über eine dynamische IP-Konfiguration verfügt, ist die Netzwerkkarte auf dem virtuellen Failovercomputer standardmäßig ebenfalls dynamisch. Sie können unter „Repliziertes Element“ > „Einstellungen“ > „Compute und Netzwerk“ > „Netzwerkschnittstellen“ eine beliebige feste IP-Adresse angeben. Sie können die Netzwerkkarte auswählen und das gewünschte Subnetz und die IP-Adresse angeben.
Traffic Manager-Integration | Unterstützt | Sie können Traffic Manager so vorkonfigurieren, dass der Datenverkehr in regelmäßigen Abständen an den Endpunkt in der Quellregion und bei einem Failover an den Endpunkt in der Zielregion weitergeleitet wird.
Mit Azure verwaltetes DNS | Unterstützt |
Benutzerdefinierter DNS  | Unterstützt |    
Nicht authentifizierter Proxy | Unterstützt | Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).    
Authentifizierter Proxy | Nicht unterstützt | Wenn der virtuelle Computer einen authentifizierten Proxy für ausgehende Verbindungen verwendet, kann er nicht mit Azure Site Recovery repliziert werden.    
Standort-zu-Standort-VPN mit lokalem Netzwerk (mit oder ohne ExpressRoute)| Unterstützt | Stellen Sie sicher, dass die UDRs und NSGs so konfiguriert sind, dass der Datenverkehr für die Sitewiederherstellung nicht lokal weitergeleitet wird. Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).  
VNet-zu-VNet-Verbindung | Unterstützt | Weitere Informationen finden Sie im [Richtliniendokument für Netzwerke](site-recovery-azure-to-azure-networking-guidance.md).  
Dienstendpunkte im virtuellen Netzwerk | Unterstützt | Azure Storage-Firewalls für virtuelle Netzwerke werden nicht unterstützt. Das Gewähren des Zugriffs auf bestimmte virtuelle Azure-Netzwerke auf Cachespeicherkonten, die zum Speichern replizierter Daten verwendet werden, wird nicht unterstützt.
Beschleunigter Netzwerkbetrieb | Unterstützt | Auf dem virtuellen Quellcomputer muss der beschleunigte Netzwerkbetrieb aktiviert sein. [Weitere Informationen](azure-vm-disaster-recovery-with-accelerated-networking.md).


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Netzwerkrichtlinien zum Replizieren von virtuellen Azure-Computern](site-recovery-azure-to-azure-networking-guidance.md)
- Schützen von Workloads durch die [Replikation virtueller Azure-Computer](site-recovery-azure-to-azure.md)
