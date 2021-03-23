---
title: Dodatek A — kody opcji protokołu DHCPv6 platformy Azure RTO NetX Duo
description: Ten rozdział zawiera opis wszystkich kodów opcji protokołu DHCPv6 NetX Duo
author: philmea
ms.author: philmea
ms.date: 06/08/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 36d673c34479ec2d476eeaa094c0dc02714ee010
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2021
ms.locfileid: "104821967"
---
# <a name="appendix-a--azure-rtos-netx-duo-dhcpv6-option-codes"></a><span data-ttu-id="c4647-103">Dodatek A — kody opcji protokołu DHCPv6 platformy Azure RTO NetX Duo</span><span class="sxs-lookup"><span data-stu-id="c4647-103">Appendix A – Azure RTOS NetX Duo DHCPv6 option codes</span></span>

| <span data-ttu-id="c4647-104">Opcja</span><span class="sxs-lookup"><span data-stu-id="c4647-104">Option</span></span>              | <span data-ttu-id="c4647-105">Kod</span><span class="sxs-lookup"><span data-stu-id="c4647-105">Code</span></span>            | <span data-ttu-id="c4647-106">Opis</span><span class="sxs-lookup"><span data-stu-id="c4647-106">Description</span></span> |
| ------------------- | ------------------- | --------------- |
| <span data-ttu-id="c4647-107">Identyfikator DUID identyfikatora klienta</span><span class="sxs-lookup"><span data-stu-id="c4647-107">Client Identifier DUID</span></span> | <span data-ttu-id="c4647-108">1</span><span class="sxs-lookup"><span data-stu-id="c4647-108">1</span></span> | <span data-ttu-id="c4647-109">Unikatowy identyfikator hosta klienta w sieci</span><span class="sxs-lookup"><span data-stu-id="c4647-109">Uniquely identifies a Client host on the network</span></span> |
| <span data-ttu-id="c4647-110">Identyfikator serwera (DUID)</span><span class="sxs-lookup"><span data-stu-id="c4647-110">Server Identifier (DUID)</span></span> | <span data-ttu-id="c4647-111">2</span><span class="sxs-lookup"><span data-stu-id="c4647-111">2</span></span> | <span data-ttu-id="c4647-112">Unikatowy identyfikator hosta DHCPv6Server w sieci</span><span class="sxs-lookup"><span data-stu-id="c4647-112">Uniquely identifies the DHCPv6Server host on the network</span></span> |
| <span data-ttu-id="c4647-113">Skojarzenie tożsamości dla adresów innych niż tymczasowe (IANA)</span><span class="sxs-lookup"><span data-stu-id="c4647-113">Identity Association for Non Temporary Addresses (IANA)</span></span> | <span data-ttu-id="c4647-114">3</span><span class="sxs-lookup"><span data-stu-id="c4647-114">3</span></span> | <span data-ttu-id="c4647-115">Parametry dla przypisania nie tymczasowego adresu IP</span><span class="sxs-lookup"><span data-stu-id="c4647-115">Parameters for a non temporary IP address assignment</span></span> |
| <span data-ttu-id="c4647-116">Skojarzenie tożsamości dla adresów tymczasowych (IATA)</span><span class="sxs-lookup"><span data-stu-id="c4647-116">Identity Association for Temporary Addresses (IATA)</span></span> | <span data-ttu-id="c4647-117">4</span><span class="sxs-lookup"><span data-stu-id="c4647-117">4</span></span> | <span data-ttu-id="c4647-118">Parametry przydziału tymczasowego adresu IP</span><span class="sxs-lookup"><span data-stu-id="c4647-118">Parameters for a temporary IP address assignment</span></span> |
| <span data-ttu-id="c4647-119">Adres IA</span><span class="sxs-lookup"><span data-stu-id="c4647-119">IA Address</span></span> | <span data-ttu-id="c4647-120">5</span><span class="sxs-lookup"><span data-stu-id="c4647-120">5</span></span> | <span data-ttu-id="c4647-121">Rzeczywisty adres IPv6 i okresy istnienia adresów IPv6, które mają być przypisane do klienta</span><span class="sxs-lookup"><span data-stu-id="c4647-121">Actual IPv6 address and IPv6 address lifetimes to be assigned to the Client</span></span> |
| <span data-ttu-id="c4647-122">Żądanie opcji</span><span class="sxs-lookup"><span data-stu-id="c4647-122">Option Request</span></span> | <span data-ttu-id="c4647-123">6</span><span class="sxs-lookup"><span data-stu-id="c4647-123">6</span></span> | <span data-ttu-id="c4647-124">Lista żądań informacji w celu uzyskania informacji sieciowych, takich jak serwer DNS i inne parametry konfiguracji sieci.</span><span class="sxs-lookup"><span data-stu-id="c4647-124">A list of information requests to obtain network information such as DNS server and other network configuration parameters.</span></span> |
| <span data-ttu-id="c4647-125">Równy</span><span class="sxs-lookup"><span data-stu-id="c4647-125">Preference</span></span> | <span data-ttu-id="c4647-126">7</span><span class="sxs-lookup"><span data-stu-id="c4647-126">7</span></span> | <span data-ttu-id="c4647-127">Uwzględniono w komunikacie anonsowania serwera klientowi wpływ na wybór serwerów przez klienta.</span><span class="sxs-lookup"><span data-stu-id="c4647-127">Included in server Advertise message to client to influence the Client’s choice of servers.</span></span> <span data-ttu-id="c4647-128">Klient musi wybrać serwer o wyższej wartości preferencji na innych serwerach.</span><span class="sxs-lookup"><span data-stu-id="c4647-128">The Client must choose a server with higher the preference value over other servers.</span></span> <span data-ttu-id="c4647-129">255 jest wartością maksymalną, natomiast zero oznacza, że klient może wybrać dowolny serwer odpowiadający na nie</span><span class="sxs-lookup"><span data-stu-id="c4647-129">255 is the maximum value, while zero indicates the client can choose any server replying back to them</span></span> |
| <span data-ttu-id="c4647-130">Czas, który upłynął</span><span class="sxs-lookup"><span data-stu-id="c4647-130">Elapsed Time</span></span> | <span data-ttu-id="c4647-131">8</span><span class="sxs-lookup"><span data-stu-id="c4647-131">8</span></span> | <span data-ttu-id="c4647-132">Zawiera czas (0,01 w sekundach), po którym klient inicjuje wymianę DHCPv6 z serwerem.</span><span class="sxs-lookup"><span data-stu-id="c4647-132">Contains the time (in 0.01 seconds) when the Client initiates the DHCPv6 exchange with the server.</span></span> <span data-ttu-id="c4647-133">Używane przez serwery pomocnicze w celu ustalenia, czy serwer podstawowy odpowiada na żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="c4647-133">Used by secondary server(s) to determine if the primary server responds in time to the Client request.</span></span> |
| <span data-ttu-id="c4647-134">Komunikat przekaźnika</span><span class="sxs-lookup"><span data-stu-id="c4647-134">Relay Message</span></span> | <span data-ttu-id="c4647-135">9</span><span class="sxs-lookup"><span data-stu-id="c4647-135">9</span></span> | <span data-ttu-id="c4647-136">Zawiera oryginalną wiadomość w komunikacie przekazywania</span><span class="sxs-lookup"><span data-stu-id="c4647-136">Contains the original message in Relay message</span></span> | 
| <span data-ttu-id="c4647-137">Authentication</span><span class="sxs-lookup"><span data-stu-id="c4647-137">Authentication</span></span> | <span data-ttu-id="c4647-138">11</span><span class="sxs-lookup"><span data-stu-id="c4647-138">11</span></span> | <span data-ttu-id="c4647-139">Zawiera informacje do uwierzytelniania tożsamości i zawartości komunikatów protokołu DHCPv6</span><span class="sxs-lookup"><span data-stu-id="c4647-139">Contains information to authenticate the identity and content of DHCPv6 messages</span></span> |
| <span data-ttu-id="c4647-140">Serwer emisji pojedynczej</span><span class="sxs-lookup"><span data-stu-id="c4647-140">Server Unicast</span></span> | <span data-ttu-id="c4647-141">12</span><span class="sxs-lookup"><span data-stu-id="c4647-141">12</span></span> | <span data-ttu-id="c4647-142">Serwer wysyła tę opcję, aby umożliwić klientowi określenie, że serwer będzie akceptować komunikaty emisji pojedynczej bezpośrednio z klienta zamiast multiemisji.</span><span class="sxs-lookup"><span data-stu-id="c4647-142">Server sends this option to let the Client know that the server will accept unicast messages directly from the Client instead of multicast.</span></span> |

<span data-ttu-id="c4647-143">Opcje IATA, Message Relay, Authentication i Server unicast nie są obsługiwane w tej wersji serwera DHCPv6 NetX Duo.</span><span class="sxs-lookup"><span data-stu-id="c4647-143">IATA, Relay Message, Authentication and Server Unicast options are not supported in this release of NetX Duo DHCPv6 Server.</span></span> <span data-ttu-id="c4647-144">Bieżący kod opcji protokołu DHCPv6 10 jest pozostawiony niezdefiniowany w dokumencie RFC 3315.</span><span class="sxs-lookup"><span data-stu-id="c4647-144">The current DHCPv6 protocol option code 10 is left undefined in RFC 3315.</span></span>