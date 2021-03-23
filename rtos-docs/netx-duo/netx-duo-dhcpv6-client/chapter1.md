---
title: Rozdział 1 — wprowadzenie do usługi Azure RTO NetX Duo klient DHCPv6
description: W przypadku sieci IPv6 protokół DHCPv6 zastępuje protokół DHCP na potrzeby dynamicznego przypisywania globalnego adresu IP z serwera DHCPv6 i oferuje większość tych samych funkcji, a także wiele ulepszeń.
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 3bf3b52c53bb26e2c9c2c736ae35817eb967f609
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2021
ms.locfileid: "104822002"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-duo-dhcpv6-client"></a><span data-ttu-id="9dbb4-103">Rozdział 1 — wprowadzenie do usługi Azure RTO NetX Duo klient DHCPv6</span><span class="sxs-lookup"><span data-stu-id="9dbb4-103">Chapter 1 - Introduction to Azure RTOS NetX Duo DHCPv6 Client</span></span>

<span data-ttu-id="9dbb4-104">W przypadku sieci IPv6 protokół DHCPv6 zastępuje protokół DHCP na potrzeby dynamicznego przypisywania globalnego adresu IP z serwera DHCPv6 i oferuje większość tych samych funkcji, a także wiele ulepszeń.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-104">In IPv6 networks, DHCPv6 replaces DHCP for dynamic global IP address assignment from a DHCPv6 Server, and offers most of the same features as well as many enhancements.</span></span> <span data-ttu-id="9dbb4-105">W tym dokumencie opisano szczegółowo, w jaki sposób interfejs API klienta DHCPv6 usługi Azure RTO NetX Duo jest używany do uzyskiwania adresów IPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-105">This document will explain in detail how the Azure RTOS NetX Duo DHCPv6 Client API is used to obtain IPv6 addresses.</span></span>

## <a name="dhcpv6-communication"></a><span data-ttu-id="9dbb4-106">Komunikacja DHCPv6</span><span class="sxs-lookup"><span data-stu-id="9dbb4-106">DHCPv6 Communication</span></span>

<span data-ttu-id="9dbb4-107">Protokół DHCPv6 używa protokołu UDP.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-107">The DHCPv6 protocol uses UDP.</span></span> <span data-ttu-id="9dbb4-108">Klient używa portu 546, a serwer używa portu 547 do wymiany komunikatów protokołu DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-108">The Client uses port 546 and the Server uses port 547 to exchange DHCPv6 messages.</span></span> <span data-ttu-id="9dbb4-109">Klient używa swojego adresu lokalnego linku dla adresu źródłowego, aby zainicjować żądania protokołu DHCPv6 dla dostępnych serwerów DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-109">The Client uses its link local address for a source address to initiate the DHCPv6 requests to the DHCPv6 server(s) available.</span></span> <span data-ttu-id="9dbb4-110">Gdy klient wysyła komunikaty przeznaczone dla wszystkich serwerów DHCPv6 w sieci, używa adresu multiemisji *All_DHCP_Relay_Agents_and_Servers* *FF02:: 01:02*.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-110">When the Client sends messages intended for all DHCPv6 servers on the network it uses the *All_DHCP_Relay_Agents_and_Servers* multicast address *FF02::01:02*.</span></span> <span data-ttu-id="9dbb4-111">Jest to zarezerwowany adres multiemisji z zakresem linków.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-111">This is a reserved, link-scoped multicast address.</span></span>

## <a name="dhcpv6-process-of-requesting-an-ipv6-address"></a><span data-ttu-id="9dbb4-112">Proces DHCPv6 żądania adresu IPv6</span><span class="sxs-lookup"><span data-stu-id="9dbb4-112">DHCPv6 Process of Requesting an IPv6 Address</span></span>

<span data-ttu-id="9dbb4-113">Aby rozpocząć proces żądania globalnego przypisania adresu IPv6, klient najpierw wysyła komunikat żądania przy użyciu usługi *nx_dhcpv6_send_solicit* :</span><span class="sxs-lookup"><span data-stu-id="9dbb4-113">To begin the process of requesting a global IPv6 address assignment, a Client first sends a SOLICIT message using the *nx_dhcpv6_send_solicit* service:</span></span>

<span data-ttu-id="9dbb4-114">**UINT nx_dhcpv6_request_solicit (NX_DHCPV6 \* dhcpv6_ptr)**</span><span class="sxs-lookup"><span data-stu-id="9dbb4-114">**UINT nx_dhcpv6_request_solicit(NX_DHCPV6 \*dhcpv6_ptr)**</span></span>

<span data-ttu-id="9dbb4-115">Ta wiadomość jest wysyłana do wszystkich serwerów przy użyciu adresu *All_DHCP_Relay_Agents_and_Servers* .</span><span class="sxs-lookup"><span data-stu-id="9dbb4-115">This message is sent to all servers using the *All_DHCP_Relay_Agents_and_Servers* address.</span></span> <span data-ttu-id="9dbb4-116">W żądaniu żądania klient może zażądać przypisania określonych adresów IPv6 jako wskazówkę do serwera.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-116">In the SOLICIT request, the Client may request the assignment of specific IPv6 address(es) as a hint to the Server.</span></span> <span data-ttu-id="9dbb4-117">Może również zażądać innych informacji o konfiguracji sieci z serwera, takiego jak serwer DNS, serwer NTP i inne opcje w żądaniu w wiadomości żądania.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-117">It can also request other network configuration information from the Server such as DNS server, NTP server and other options in the Option Request in the SOLICIT message.</span></span>

<span data-ttu-id="9dbb4-118">Serwer DHCPv6, który może obsłużyć żądanie klienta, odpowiada za pomocą wiadomości ANONSOWANej zawierającej adresy IPv6, które można przypisać do klienta, czas dzierżawy adresu IPv6 i wszelkie dodatkowe informacje wymagane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-118">A DHCPv6 Server that can service a Client request responds with an ADVERTISE message containing the IPv6 address(es) it can assign to the Client, the IPv6 address lease time and any additional information requested by the Client.</span></span> <span data-ttu-id="9dbb4-119">Protokół klienta DHCPv6 wymaga, aby klient czekał przez pewien czas na otrzymywanie ANONSów komunikatów ze wszystkich serwerów DHCPv6 w sieci.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-119">The DHCPv6 Client protocol requires the Client to wait for a period of time to receive ADVERTISE messages from all DHCPv6 Servers on the network.</span></span> <span data-ttu-id="9dbb4-120">Przetwarza wstępnie każdy komunikat ANONSOWANy jako prawidłowy komunikat i skanuje dane opcji dla różnych parametrów protokołu DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-120">It pre-processes each ADVERTISE message to be a valid message, and scans the option data for various DHCPv6 parameters.</span></span> <span data-ttu-id="9dbb4-121">Sprawdza również wartość preferencji w preferencjach, jeśli jest ona dostarczana przez serwer.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-121">It also checks the Preference value in the Preference Option, if supplied by the Server.</span></span> <span data-ttu-id="9dbb4-122">W przypadku odebrania więcej niż jednego komunikatu ANONSu NetX klient DHCPv6 wybiera serwer o najwyższej wartości preferencji otrzymanej na koniec okresu oczekiwania.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-122">If more than one ADVERTISE message is received, the NetX DHCPv6 Client chooses the Server with the highest preference value received by the end of the wait period.</span></span>

<span data-ttu-id="9dbb4-123">Wyjątek polega na tym, że klient otrzymuje komunikat ANONSu z wartością preferencji 255.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-123">The exception is if the Client receives an ADVERTISE message with a preference value of 255.</span></span> <span data-ttu-id="9dbb4-124">Akceptuje on komunikat natychmiast i odrzuca wszystkie kolejne komunikaty ANONSOWANe.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-124">It accepts that message immediately and discards all subsequent ADVERTISE messages.</span></span>

<span data-ttu-id="9dbb4-125">Okres oczekiwania jest definiowany jako okres retransmisji, który czeka klient DHCPv6 przed wysłaniem kolejnego komunikatu żądania, jeśli nie otrzymał odpowiedzi z żadnego serwera.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-125">The wait period is defined as the retransmission period that the DHCPv6 Client waits before sending another SOLICIT message if it has not received a response from any Server.</span></span> <span data-ttu-id="9dbb4-126">Początkowy limit czasu ponownej transmisji w stanie żądania jest definiowany przez protokół DHCPv6 opisany w dokumencie RFC 3315 to 1 sekunda.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-126">The initial retransmission timeout in the SOLICIT state is defined by to the DHCPv6 protocol described in RFC 3315 to be 1 second.</span></span> <span data-ttu-id="9dbb4-127">Kolejne interwały ponownej transmisji, jeśli klient DHCPv6 nie otrzymuje prawidłowej odpowiedzi na serwerze, zostanie podwojony do maksymalnie 120 sekund.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-127">Subsequent retransmission intervals, if the DHCPv6 Client fails to receive a valid Server response, are doubled up to a maximum of 120 seconds.</span></span>

<span data-ttu-id="9dbb4-128">Po wybraniu serwera klient wyodrębnia dane z wiadomości ANONSOWANej i wysyła komunikat żądania z powrotem do serwera w celu zaakceptowania przypisanych informacji o adresie i czasów dzierżawy.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-128">Having chosen the Server, the Client extracts data from the ADVERTISE message and sends a REQUEST message back to the Server to accept the assigned address information and lease times.</span></span> <span data-ttu-id="9dbb4-129">Serwer reaguje na komunikat odpowiedzi, aby potwierdzić, że adresy IPv6 klienta są zarejestrowane na serwerze przypisanym do klienta.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-129">The Server responds with a REPLY message to confirm the Client IPv6 address(es) are registered with the Server as assigned to the Client.</span></span>

<span data-ttu-id="9dbb4-130">Klient DHCPv6 rejestruje przypisane adresy IPv6 z wystąpieniem IP (np. NetX Duo).</span><span class="sxs-lookup"><span data-stu-id="9dbb4-130">The DHCPv6 Client registers the assigned IPv6 address(es) with the IP instance (e.g NetX Duo).</span></span> <span data-ttu-id="9dbb4-131">Jeśli skonfigurowano dla protokołu wykrywania duplikatów adresów (DAD) (domyślnie włączony), NetX Duo automatycznie wyśle komunikaty żądania sąsiada, aby sprawdzić, czy przypisane adresy są unikatowe w sieci.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-131">If configured for the Duplicate Address Detection (DAD) protocol (enabled by default), NetX Duo will automatically send Neighbor Solicit messages to verify the assigned address(es) are unique on the network.</span></span> <span data-ttu-id="9dbb4-132">Jeśli tak, powiadamia klienta protokołu DHCPv6, gdy każdy przypisany adres zostanie podwyższony do nieprawidłowej ważności.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-132">If so, it notifies the DHCPv6 Client when the each assigned address has been promoted from TENTATIVE to VALID.</span></span> <span data-ttu-id="9dbb4-133">Klient DHCPv6 zostanie podwyższony do stanu POWIĄZANEgo, a urządzenie może używać tego adresu IPv6 do wysyłania i przesyłania komunikatów.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-133">The DHCPv6 Client is promoted to the BOUND state and the device may use that IPv6 address to send and transmit messages.</span></span> <span data-ttu-id="9dbb4-134">W przypadku niepowodzenia protokołu DAD NetX Duo powiadamia klienta protokołu DHCPv6, a klient DHCPv6 wysyła komunikat odrzucania dla adresów IPv6 przypisanych do serwera i resetuje stan klienta DHCPv6 do stanu INIT.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-134">If the DAD protocol fails, NetX Duo notifies the DHCPv6 Client and the DHCPv6 Client sends a DECLINE message for the IPv6 address(es) assigned back to the Server and resets the DHCPv6 Client state to the INIT state.</span></span>

### <a name="notification-of-successful-address-assignment-and-validation"></a><span data-ttu-id="9dbb4-135">Powiadomienie o pomyślnym przypisaniu i sprawdzeniu adresu</span><span class="sxs-lookup"><span data-stu-id="9dbb4-135">Notification of Successful Address Assignment and Validation</span></span>

<span data-ttu-id="9dbb4-136">Aplikacja może określić wynik żądania adresu klienta DHCPv6 od zmiany stanu, jeśli klient DHCPv6 zostanie skonfigurowany przy użyciu wywołania zwrotnego zmiany stanu w usłudze *nx_dhcpv6_client_create* .</span><span class="sxs-lookup"><span data-stu-id="9dbb4-136">The application can determine the result of the DHCPv6 Client address solicitation from the state changes if the DHCPv6 Client is configured with the state change callback in the *nx_dhcpv6_client_create* service.</span></span> <span data-ttu-id="9dbb4-137">Jeśli odbierze odpowiedź od serwera, zaobserwowane zmiany stanu są z żądania do INIT.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-137">If it receives no response from the Server the state change observed is from SOLICIT to INIT.</span></span> <span data-ttu-id="9dbb4-138">Jeśli odebrano odpowiedź z serwera, ale serwer nie może przypisać adresu, aplikacja zostanie powiadomiona przez wywołanie zwrotne błędu serwera klienta DHCPv6, jeśli zostało skonfigurowane z jednym (również w *nx_dhcpv6_client_create).*</span><span class="sxs-lookup"><span data-stu-id="9dbb4-138">If it received a response from the Server but the Server is unable to assign the address, the application will be notified by the DHCPv6 Client server error callback if configured with one (also in *nx_dhcpv6_client_create).*</span></span> <span data-ttu-id="9dbb4-139">Jeśli klient osiągnął stan związany, ale nie zakończył się sprawdzaniem DAD, zobaczy zmianę stanu z powiązania z elementem INIT.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-139">If the Client achieved the BOUND state but then failed the DAD check, it will see a state change from BOUND to INIT.</span></span> <span data-ttu-id="9dbb4-140">Należy pamiętać, że aplikacja, która jest włączona dla DAD, musi przekroczyć czas dla nieprawidłowego sprawdzenia po rozpoczęciu procesu żądania.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-140">Note that an application that is enabled for DAD must allow time for the DAD check after starting the request process.</span></span> <span data-ttu-id="9dbb4-141">Zwykle jest to około 400-500 taktów (4-5 s w większości przypadków).</span><span class="sxs-lookup"><span data-stu-id="9dbb4-141">Typically this is about 400-500 ticks (4-5 seconds in most cases).</span></span>

### <a name="relinquishing-an-ipv6-address"></a><span data-ttu-id="9dbb4-142">Zwalnianie adresu IPv6</span><span class="sxs-lookup"><span data-stu-id="9dbb4-142">Relinquishing an IPv6 Address</span></span>

<span data-ttu-id="9dbb4-143">Jeśli i kiedy klient musi zwolnić przypisany adres IPv6, informuje serwer DHCPv6 przez wywołanie usługi *nx_dhcpv6_request_release* :</span><span class="sxs-lookup"><span data-stu-id="9dbb4-143">If and when the Client needs to release an assigned IPv6 address, it informs the DHCPv6 server by calling the *nx_dhcpv6_request_release* service:</span></span>

### <a name="uint-nx_dhcpv6_request_releasenx_dhcpv6-dhcpv6_ptr"></a><span data-ttu-id="9dbb4-144">UINT nx_dhcpv6_request_release (NX_DHCPV6 \* dhcpv6_ptr)</span><span class="sxs-lookup"><span data-stu-id="9dbb4-144">UINT nx_dhcpv6_request_release(NX_DHCPV6 \*dhcpv6_ptr)</span></span>

<span data-ttu-id="9dbb4-145">Klient DHCPv6 wysyła komunikat o wydanie emisji pojedynczej zawierający przypisane adresy do serwera, który przypisał adres, i czeka na odpowiedź potwierdzające, że serwer odebrał komunikat.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-145">The DHCPv6 Client sends a unicast RELEASE message containing the assigned addresses to the Server who assigned the address and waits for a REPLY confirming the Server received the message.</span></span>

<span data-ttu-id="9dbb4-146">Uwaga: istnieje inny proces dla klienta, który jest włączony, ale planuje dalsze używanie przypisanych adresów IPv6 przy ponownym uruchomieniu.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-146">Note: There is a different process for the Client that is powering down but plans to continue using the assigned IPv6 addresses on reboot.</span></span> <span data-ttu-id="9dbb4-147">Komunikat o ZWOLNIeniu nie jest wysyłany na wyłączność, chyba że planuje zażądanie nowego adresu.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-147">It does not send the RELEASE message on powering down unless it plans to request a new address on power up.</span></span> <span data-ttu-id="9dbb4-148">Aby uzyskać wyjaśnienie tej sytuacji, zobacz "wymagania dotyczące pamięci nietrwałej".</span><span class="sxs-lookup"><span data-stu-id="9dbb4-148">See “Non Volatile Memory Requirements” for explanation of this situation.</span></span>

### <a name="dhcpv6-lease-timeouts"></a><span data-ttu-id="9dbb4-149">Limity czasu dzierżawy DHCPv6</span><span class="sxs-lookup"><span data-stu-id="9dbb4-149">DHCPv6 Lease Timeouts</span></span>

<span data-ttu-id="9dbb4-150">Dzierżawa IPv6 przypisana przez serwer zawiera dwa parametry limitu czasu (T1 i T2) w każdym skojarzeniu tożsamości — bez adresów tymczasowych (IANA).</span><span class="sxs-lookup"><span data-stu-id="9dbb4-150">The IPv6 lease assigned by the Server contains two timeout parameters, T1 and T2 in each Identity Association – Non Temporary Addresses (IANA) block.</span></span> <span data-ttu-id="9dbb4-151">Organizacja IANA jest opisana w innym miejscu tego podręcznika użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-151">An IANA is described in elsewhere in this User Guide.</span></span> <span data-ttu-id="9dbb4-152">Jeśli czas, który upłynął od momentu powiązania klienta DHCPv6 z przypisanym adresem IPv6 równa T1, klient DHCPv6 automatycznie rozpocznie odnowienie adresu IPv6 przez wysłanie komunikatu o ODNOWIENIu.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-152">If the elapsed time from when the DHCPv6 Client was bound to the assigned IPv6 address equals T1, the DHCPv6 Client automatically starts renewing the IPv6 address by sending a RENEW message.</span></span> <span data-ttu-id="9dbb4-153">Jeśli czas, który upłynął, jest równy T2, klient DHCPv6 automatycznie wyśle komunikat o konieczności ponownego powiązania, jeśli nie otrzymał odpowiedzi na żądania ODNOWIENIa.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-153">If the elapsed time equals T2, DHCPv6 Client automatically sends a REBIND message if it received no responses to its RENEW requests.</span></span>

<span data-ttu-id="9dbb4-154">Dwa inne parametry dzierżawy IPv6, preferowany i prawidłowy okres istnienia są przypisywane do każdego bloku skojarzenia tożsamości zawartego w bloku IANA.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-154">Two other IPv6 lease parameters, preferred and valid lifetime, are assigned with each Identity Association (IA) block contained in the IANA block.</span></span> <span data-ttu-id="9dbb4-155">Preferowany i prawidłowy okres istnienia to w przypadku, gdy przypisany adres IPv6 jest przestarzały lub nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-155">The preferred and valid lifetimes are when the assigned IPv6 address is deprecated or invalid, respectively.</span></span> <span data-ttu-id="9dbb4-156">T1 musi być krótszy niż preferowany okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-156">T1 must be less than the preferred lifetime.</span></span> <span data-ttu-id="9dbb4-157">Wartość T2 musi być mniejsza niż prawidłowy okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-157">T2 must be less than the valid lifetime.</span></span>

### <a name="ip-thread-task-requirements"></a><span data-ttu-id="9dbb4-158">Wymagania zadania wątku IP</span><span class="sxs-lookup"><span data-stu-id="9dbb4-158">IP Thread Task Requirements</span></span>

<span data-ttu-id="9dbb4-159">Klient DHCPv6 NetX Duo wymaga utworzenia wystąpienia adresu IP w programie NetX Duo przed utworzeniem klienta DHCPv6 do korzystania z usług klienta DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-159">The NetX Duo DHCPv6 Client requires creation of a NetX Duo IP instance previous to creating the DHCPv6 Client to use DHCPv6 Client services.</span></span>

<span data-ttu-id="9dbb4-160">Protokoły UDP, IPv6 i ICMPv6 muszą być włączone w wystąpieniu IP przed użyciem klienta DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-160">UDP, IPv6, and ICMPv6 must be enabled on the IP instance prior to the using DHCPv6 Client.</span></span>

  - <span data-ttu-id="9dbb4-161">*nx_udp_enable*</span><span class="sxs-lookup"><span data-stu-id="9dbb4-161">*nx_udp_enable*</span></span>

  - <span data-ttu-id="9dbb4-162">*nxd_ipv6_enable*</span><span class="sxs-lookup"><span data-stu-id="9dbb4-162">*nxd_ipv6_enable*</span></span>

  - <span data-ttu-id="9dbb4-163">*nxd_icmp_enable*</span><span class="sxs-lookup"><span data-stu-id="9dbb4-163">*nxd_icmp_enable*</span></span>

### <a name="packet-pool-requirements"></a><span data-ttu-id="9dbb4-164">Wymagania dotyczące puli pakietów</span><span class="sxs-lookup"><span data-stu-id="9dbb4-164">Packet Pool Requirements</span></span>

<span data-ttu-id="9dbb4-165">Tworzenie klienta DHCPv6 w NetX Duo wymaga wcześniej utworzonej puli pakietów do wysyłania komunikatów protokołu DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-165">NetX Duo DHCPv6 Client creation requires a previously created packet pool for sending DHCPv6 messages.</span></span> <span data-ttu-id="9dbb4-166">Rozmiar puli pakietów pod względem ładunku pakietu i liczby dostępnych pakietów jest konfigurowany przez użytkownika i zależy od przewidywanej ilości komunikatów protokołu DHCPv6 oraz innych przesyłanych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-166">The size of the packet pool in terms of packet payload and number of packets available is user configurable, and depends on the anticipated volume of DHCPv6 messages and other transmissions the application will be sending.</span></span>

<span data-ttu-id="9dbb4-167">Typowy komunikat protokołu DHCPv6 ma około 200 bajtów w zależności od liczby adresów IA i opcji protokołu DHCPv6 żądanych przez klienta.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-167">A typical DHCPv6 message is about 200 bytes depending on the number of IA addresses and DHCPv6 options requested by the Client.</span></span>

### <a name="network-requirements"></a><span data-ttu-id="9dbb4-168">Wymagania dotyczące sieci</span><span class="sxs-lookup"><span data-stu-id="9dbb4-168">Network Requirements</span></span>

<span data-ttu-id="9dbb4-169">Klient DHCPv6 NetX Duo wymaga utworzenia gniazda UDP powiązanego z portem 546.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-169">The NetX Duo DHCPv6 Client requires the creation of UDP socket bound to port 546.</span></span> <span data-ttu-id="9dbb4-170">Gniazdo jest tworzone podczas tworzenia zadania klienta DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-170">The socket is created when the DHCPv6 Client task is created.</span></span>

### <a name="non-volatile-memory-requirements"></a><span data-ttu-id="9dbb4-171">Wymagania dotyczące pamięci nietrwałej</span><span class="sxs-lookup"><span data-stu-id="9dbb4-171">Non Volatile Memory Requirements</span></span>

<span data-ttu-id="9dbb4-172">Jeśli klient DHCPv6 zwolni dzierżawę protokołu IPv6 przy użyciu serwera DHCPv6 podczas włączania i żąda nowych adresów IPv6 po ponownym uruchomieniu, nietrwały Magazyn pamięci nie jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-172">If a DHCPv6 Client releases its IPv6 lease with the DHCPv6 Server when powering down, and requests new IPv6 address(es) on reboot, then non volatile memory storage is not required.</span></span> <span data-ttu-id="9dbb4-173">Jeśli klient chce nadal korzystać z przypisanej dzierżawy, musi przechowywać pewne informacje o kliencie DHCPv6 do nietrwałej pamięci w ramach ponownych uruchomień.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-173">If a Client wishes to continue using its assigned lease, it must store certain information about the DHCPv6 Client to non volatile memory across reboots.</span></span>

<span data-ttu-id="9dbb4-174">Wymagania dotyczące pamięci nieulotnej i interfejsu API klienta protokołu DHCPv6 zostały omówione w dalszej części w temacie **Korzystanie z klienta Dhcpv6 NetX Duo** w rozdziale dwa.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-174">Non volatile memory requirements and the DHCPv6 Client API are discussed further in **Using the NetX Duo DHCPv6 Client** in Chapter Two.</span></span>

### <a name="netx-duo-dhcpv6-client-limitations"></a><span data-ttu-id="9dbb4-175">Ograniczenia klienta DHCPv6 NetX Duo</span><span class="sxs-lookup"><span data-stu-id="9dbb4-175">NetX Duo DHCPv6 Client Limitations</span></span>

<span data-ttu-id="9dbb4-176">Bieżąca wersja klienta DHCPv6 NetX Duo ma następujące ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="9dbb4-176">The current release of the NetX Duo DHCPv6 Client has the following limitations:</span></span>

  - <span data-ttu-id="9dbb4-177">Klient DHCPv6 NetX Duo nie obsługuje opcji serwer emisji pojedynczej do wysyłania komunikatów protokołu DHCPv6 emisji pojedynczej do serwera DHCPv6, nawet jeśli serwer wskazuje, że jest to dozwolone.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-177">NetX Duo DHCPv6 Client does not support the Server Unicast option for sending unicast DHCPv6 messages to the DHCPv6 Server even if the Server indicates this is permitted.</span></span>

  - <span data-ttu-id="9dbb4-178">Klient DHCPv6 NetX Duo nie obsługuje żądania ponownego skonfigurowania, w którym serwer inicjuje zmiany adresów IPv6 klientom w sieci.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-178">NetX Duo DHCPv6 Client does not support the Reconfigure request in which a Server initiates IPv6 address changes to the Clients on the network.</span></span>

  - <span data-ttu-id="9dbb4-179">Klient DHCPv6 NetX Duo nie obsługuje formatu przedsiębiorstwa dla bloku kontroli unikatowy identyfikator protokołu DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-179">NetX Duo DHCPv6 Client does not support the Enterprise format for the DHCPv6 Unique Identifier control block.</span></span> <span data-ttu-id="9dbb4-180">Obsługuje tylko warstwy łączy i warstwy łączy oraz format czasu.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-180">It only supports Link Layer and Link Layer Plus Time format.</span></span>

  - <span data-ttu-id="9dbb4-181">Klient DHCPv6 NetX Duo nie obsługuje żądań adresów tymczasowych skojarzenia (), ale obsługuje żądania opcji innych niż tymczasowe (IANA).</span><span class="sxs-lookup"><span data-stu-id="9dbb4-181">NetX Duo DHCPv6 Client does not support Temporary Association (TA) address requests, but does support Non Temporary (IANA) option requests.</span></span>

## <a name="multihome-and-multiple-address-support"></a><span data-ttu-id="9dbb4-182">Obsługa wielu użytkowników i adresów</span><span class="sxs-lookup"><span data-stu-id="9dbb4-182">Multihome and Multiple Address Support</span></span>

<span data-ttu-id="9dbb4-183">Klient DHCPv6 obsługuje wiele interfejsów i wiele adresów na interfejs.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-183">The DHCPv6 Client supports multiple interfaces and multiple addresses per interface.</span></span> <span data-ttu-id="9dbb4-184">Usługa klienta DHCPv6 *nx_dhcpv6_client_set_interface* umożliwia aplikacji klienckiej Ustawianie interfejsu sieciowego, na którym aplikacja będzie komunikować się z serwerem DHCPv6.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-184">The DHCPv6 Client service, *nx_dhcpv6_client_set_interface* enables the Client application to set the network interface on which the application will be communicating with the DHCPv6 Server.</span></span> <span data-ttu-id="9dbb4-185">Klient DHCPv6 domyślnie jest interfejsem podstawowym (indeks zero).</span><span class="sxs-lookup"><span data-stu-id="9dbb4-185">The DHCPv6 Client defaults to the primary interface (index zero).</span></span>

<span data-ttu-id="9dbb4-186">W przypadku wielu adresów na interfejs klient DHCPv6 utrzymuje wewnętrzną listę adresów, zaczynając od indeksu 0.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-186">For multiple addresses per interface, the DHCPv6 Client keeps an internal list of addresses starting at index 0.</span></span> <span data-ttu-id="9dbb4-187">Należy pamiętać, że ten sam adres zarejestrowany z klientem DHCPv6 nie musi znajdować się w tym samym indeksie w tabeli IP adresów interfejsów.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-187">Note that the same address registered with the DHCPv6 Client may not necessarily be located at the same index in the IP table of interface addresses.</span></span>

<span data-ttu-id="9dbb4-188">W przypadku usług klienta DHCPv6, które pobierają informacje o dzierżawie adresów IPv6 klienta, niektóre wymagają określenia indeksu adresu.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-188">For DHCPv6 Client services that retrieve information about the Client IPv6 address lease, some require an address index to be specified.</span></span> <span data-ttu-id="9dbb4-189">Poniżej przedstawiono przykład uzyskiwania preferowanych i prawidłowych okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="9dbb4-189">An example for obtaining the preferred and valid lifetimes is shown below:</span></span>

```C
UINT nx_dhcpv6_get_valid_ip_address_lease_time(NX_DHCPV6 *dhcpv6_ptr, 
                                              UINT address_index, 
                                              NXD_ADDRESS *ip_address,
                                              ULONG preferred_lifetime, 
                                              ULONG *valid_lifetime)
```

<span data-ttu-id="9dbb4-190">Aplikacja kliencka może również pobrać liczbę prawidłowych adresów IPv6 przypisanych z usługi *nx_dhcpv6_get_valid_ip_address_count* :</span><span class="sxs-lookup"><span data-stu-id="9dbb4-190">The Client application can also retrieve the number of valid IPv6 addresses assigned from the *nx_dhcpv6_get_valid_ip_address_count* service:</span></span>

```C
UINT nx_dhcpv6_get_valid_ip_address_count(NX_DHCPV6 *dhcpv6_ptr, 
                                          UINT *address_count)
```

<span data-ttu-id="9dbb4-191">Starsze usługi klienta DHCPv6 utworzone przed obsługą wielu adresów w NetX Duo nie pobierają indeksu adresu.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-191">Legacy DHCPv6 Client services which were created before multiple addresses were supported in NetX Duo do not take an address index.</span></span> <span data-ttu-id="9dbb4-192">W związku z tym te usługi są z tego powodu wykonywane z podstawowego globalnego adresu IA, niezależnie od tego, jak wiele adresów IA jest przypisanych do klienta.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-192">Therefore, with these services, the data requested is taken from the primary global IA address, regardless how many IA addresses are assigned to the Client.</span></span> <span data-ttu-id="9dbb4-193">Przykład przedstawiono poniżej:</span><span class="sxs-lookup"><span data-stu-id="9dbb4-193">An example is shown below:</span></span>

```C
UINT nx_dhcpv6_get_IP_address(NX_DHCPV6 *dhcpv6_ptr, 
                              NXD_ADDRESS *ip_address)
```

## <a name="netx-duo-dhcpv6-client-callback-functions"></a><span data-ttu-id="9dbb4-194">Funkcje wywołania zwrotnego klienta DHCPv6 NetX Duo</span><span class="sxs-lookup"><span data-stu-id="9dbb4-194">NetX Duo DHCPv6 Client Callback Functions</span></span>

<span data-ttu-id="9dbb4-195">*nx_dhcpv6_state_change_callback*</span><span class="sxs-lookup"><span data-stu-id="9dbb4-195">*nx_dhcpv6_state_change_callback*</span></span>

<span data-ttu-id="9dbb4-196">Gdy klient DHCPv6 zmieni stan protokołu DHCPv6 w wyniku przetwarzania żądania DHCPv6, powiadamia aplikację za pomocą tej funkcji wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-196">When the DHCPv6 Client changes to a new DHCPv6 state as a result of processing a DHCPv6 request, it notifies the application with this callback function.</span></span>

<span data-ttu-id="9dbb4-197">*nx_dhcpv6_server_error_handler*</span><span class="sxs-lookup"><span data-stu-id="9dbb4-197">*nx_dhcpv6_server_error_handler*</span></span>

<span data-ttu-id="9dbb4-198">Gdy klient DHCPv6 odbiera odpowiedź serwera zawierającą opcję *stanu* o stanie innym niż zero (niepowodzenie), powiadamia aplikację za pomocą tego wywołania zwrotnego, które zawiera kod stanu błędu serwera.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-198">When the DHCPv6 Client receives a Server reply containing a *Status* option with a non-zero (non successful) status, it notifies the application with this callback which includes the Server error status code.</span></span>

<span data-ttu-id="9dbb4-199">Uwaga: ponieważ te funkcje wywołania zwrotnego są wywoływane z zadania wątku klienta protokołu DHCPv6, aplikacja kliencka nie może wywołać żadnych usług klienta DHCPv6 NetX Duo, które wymagają kontroli muteksów klienta DHCPv6, takich jak *nx_dhcpv6_start, nx_dhcpv6_stop* i dowolny interfejs API, który wysyła wiadomości bezpośrednio z wywołania zwrotnego, np. *nx_dhcpv6_request_release*.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-199">Note: Since these callback functions are called from the DHCPv6 Client thread task, the Client application must NOT call any NetX Duo DHCPv6 Client services that require mutex control of the DHCPv6 Client such as *nx_dhcpv6_start, nx_dhcpv6_stop,* and any of the API that send messages directly from the callback e.g. *nx_dhcpv6_request_release*.</span></span>

## <a name="dhcpv6-rfcs"></a><span data-ttu-id="9dbb4-200">Specyfikacje RFC protokołu DHCPv6</span><span class="sxs-lookup"><span data-stu-id="9dbb4-200">DHCPv6 RFCs</span></span>

<span data-ttu-id="9dbb4-201">Usługa DHCP NetX Duo jest zgodna z RFC3315, RFC3646 i powiązanymi specyfikacjami RFC.</span><span class="sxs-lookup"><span data-stu-id="9dbb4-201">NetX Duo DHCP is compliant with RFC3315, RFC3646, and related RFCs.</span></span>