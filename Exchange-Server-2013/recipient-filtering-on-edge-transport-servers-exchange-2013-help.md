﻿---
title: 'Edge 전송 서버의 받는 사람 필터링: Exchange 2013 Help'
TOCTitle: Edge 전송 서버의 받는 사람 필터링
ms:assetid: 994eefd9-3903-41e6-a882-1e333d6d2d18
ms:mtpsurl: https://technet.microsoft.com/ko-kr/library/Bb123891(v=EXCHG.150)
ms:contentKeyID: 50483753
ms.date: 05/22/2018
mtps_version: v=EXCHG.150
ms.translationtype: MT
---

# Edge 전송 서버의 받는 사람 필터링

 

_**적용 대상:** Exchange Server 2013_

_**마지막으로 수정된 항목:** 2014-05-07_

받는 사람 필터링은 RCPT TO SMTP 헤더를 사용하여 필요한 경우 인바운드 메시지에 대해 수행할 작업을 결정하는 Microsoft Exchange Server 2013의 스팸 방지 기능입니다. 받는 사람 필터링은 받는 사람 필터 에이전트에 의해 수행됩니다.

받는 사람 필터 에이전트는 조직에서 받는 사람의 특성에 따라 메시지를 차단합니다. 받는 사람 필터 에이전트를 사용하면 다음과 같은 시나리오에서 메시지가 허용되지 않도록 할 수 있습니다.

  - **존재하지 않는 받는 사람**   조직의 주소록에 없는 받는 사람에게 메시지가 배달되지 않도록 할 수 있습니다. 예를 들어 administrator@contoso.com이나 support@contoso.com과 같이 빈번하게 잘못 사용되는 계정 이름에 대한 배달을 중지할 수 있습니다.

  - **제한된 메일 그룹**   내부 사용자만 사용해야 하는 메일 그룹에 인터넷 메일이 배달되지 않도록 할 수 있습니다.

  - **인터넷에서 메시지를 받지 않아야 하는 사서함**   일반적으로 조직 내부에서 사용되는 특정 사서함이나 별칭(예: Helpdesk)에 인터넷 메일이 배달되지 않도록 할 수 있습니다.

받는 사람 필터 에이전트는 다음 두 데이터 원본 중 하나 또는 모두에 저장되어 있는 받는 사람에 작동합니다.

  - **차단된 받는 사람 목록**   인터넷에서 들어오는 메시지를 수신하지 않아야 하는 받는 사람의 목록이며 관리자가 정의합니다.

  - **받는 사람 조회**   Active Directory를 쿼리하여 받는 사람이 조직에 있는지 확인합니다. Edge 전송 서버에서 받는 사람 조회는 AD LDS(Active Directory Lightweight Directory Services)의 로컬 인스턴스에 EdgeSync가 제공하는 Active Directory 정보에 액세스해야 합니다.

받는 사람 필터 에이전트를 사용하도록 설정하면 받는 사람의 특성에 따라 인바운드 메시지에 대해 다음 작업 중 하나가 수행됩니다. 이러한 받는 사람은 RCPT TO 헤더로 표시됩니다.

  - 인바운드 메시지에 차단된 받는 사람 목록에 포함된 받는 사람이 있으면 Exchange 서버는 보내는 서버에 `550 5.1.1 User unknown` SMTP 세션 오류를 전송합니다.

  - 인바운드 메시지에 받는 사람 조회의 받는 사람 중 누구와도 일치하지 않는 받는 사람이 포함되어 있으면 Exchange 서버는 보내는 서버에 `550 5.1.1 User unknown` SMTP 세션 오류를 전송합니다.

  - 받는 사람이 받는 사람 차단 목록에는 없고 받는 사람 조회에는 있는 경우 Exchange 서버는 보내는 서버에 `250 2.1.5 Recipient OK` SMTP 응답을 보내고 체인의 그 다음 스팸 방지 에이전트가 해당 메시지를 처리합니다.

**목차**

Configuring recipient lookup

Tarpitting functionality

Multiple namespaces

## 받는 사람 조회 구성

스팸을 줄이는 가장 효과적인 방법 중 하나는 인터넷에서 들어오는 인바운드 메시지를 수락하기 전에 받는 사람을 확인하는 것입니다. Exchange 관리 셸에서 **Set-RecipientFilterConfig** cmdlet을 사용하면 Exchange 조직에 존재하지 않는 받는 사람에게 전송된 메시지의 차단과 특정 받는 사람의 차단을 사용하도록 설정할 수 있습니다. 자세한 내용은 [Edge 전송 서버의 받는 사람 필터링 관리](manage-recipient-filtering-on-edge-transport-servers-exchange-2013-help.md)를 참조하십시오.

경계 네트워크에 Edge 전송 서버가 설치된 경우에는 Edge 전송 서버에서 실행되는 AD LDS 인스턴스가 Active Directory에 동기화되도록 구성하는 것이 좋습니다. 기본적으로 AD LDS는 Edge 전송 서버에 설치 및 구성됩니다. 하지만 Edge 전송 서버를 조직에 가입시켜 AD LDS가 Active Directory 도메인 결합 글로벌 카탈로그 서버와 통신하도록 구성해야 합니다. 자세한 내용은 [Exchange 2010 또는 2007 Edge 전송 서버에서 Exchange 2013 사용](use-an-exchange-2010-or-2007-edge-transport-server-in-exchange-2013-exchange-2013-help.md)을 참조하십시오.

맨 위로 이동

## 타피팅 기능

받는 사람 조회 기능을 사용하면 보내는 서버에서 전자 메일 주소가 올바른지 여부를 확인할 수 있습니다. 앞에서 설명한 대로 인바운드 메시지의 받는 사람이 알려진 받는 사람인 경우 Exchange 서버는 보내는 서버에 `250 2.1.5 Recipient OK` 응답을 보냅니다. 이 기능은 디렉터리 수집 공격에 매우 이상적인 환경을 제공합니다.

*디렉터리 수집 공격*은 스팸 데이터베이스에 추가할 수 있도록 특정 조직에서 올바른 전자 메일 주소를 수집하는 시도입니다. 모든 스팸의 수입은 사람들이 전자 메일 메시지를 열어보도록 만드는 데 달려 있으므로 현재 사용 중인 것으로 확인된 주소는 악의적인 사용자나 *스팸 발송자*가 대가를 지불할 만한 유용한 대상이 됩니다. SMTP 프로토콜에서는 알려진 보낸 사람과 알 수 없는 보낸 사람에 대한 피드백이 제공되므로 스팸 발송자는 일반적인 이름이나 사전의 단어를 사용하여 특정 도메인의 전자 메일 주소를 만드는 자동화 프로그램을 작성할 수 있습니다. 이러한 프로그램은 `250 2.1.5 Recipient OK` SMTP 응답을 반환하는 모든 전자 메일 주소를 수집하고 `550 5.1.1 User unknown` SMTP 세션 오류를 반환하는 모든 전자 메일 주소를 버립니다. 그러면 스팸 발송자가 유효한 전자 메일 주소를 판매하거나 원치 않는 메시지의 받는 사람으로 사용할 수 있습니다.

디렉터리 수집 공격에 대응하기 위해 Exchange 2013에 타피팅 기능이 추가되었습니다. *타피팅*은 대용량 스팸이나 원치 않는 다른 메시지를 나타내는 특정 SMTP 통신 패턴의 서버 응답을 인위적으로 지연시키는 기능입니다. 타피팅은 이러한 전자 메일 트래픽에 대한 통신 처리를 늦춰 스팸 메일을 보내는 사람이나 조직의 스팸 전송 비용을 증가시키기 위한 것입니다. 즉, 타피팅은 수집 공격을 효율적으로 자동화하는 데 너무 많은 비용이 소모되도록 만듭니다.

타피팅이 구성되어 있지 않으면 받는 사람 조회에서 받는 사람을 찾을 수 없는 경우 Exchange 서버가 즉시 보낸 사람에게 `550 5.1.1 User unknown` SMTP 세션 오류를 반환합니다. 그러나 타피팅이 구성되어 있으면 SMTP가 지정된 시간(초 단위) 동안 기다린 후에 `550 5.1.1 User unknown` 오류를 반환합니다. SMTP 세션에서의 이러한 지연은 디렉터리 수집 공격의 자동화를 어렵게 만들고 스팸 발송자의 비용 효과성을 떨어뜨립니다. 기본적으로 수신 커넥터에 대한 타피팅은 5초로 구성됩니다.

SMTP가 `550 5.1.1 User unknown` 오류를 반환하기 전의 지연 시간을 구성하려면 **Set-ReceiveConnector** cmdlet에 *TarpitInterval* 매개 변수를 사용하여 타피팅 간격을 설정합니다. 구문은 다음과 같습니다.

    Set-ReceiveConnector <Receive Connector> -TarpitInterval <00:00:00 to 00:10:00>

기본값은 `00:00:05` 또는 5초입니다. Edge 전송 서버의 기본 수신 커넥터는 `Default internal receive connector <server name>`입니다.

타피팅 간격을 변경할 때는 주의해야 합니다. 간격이 너무 짧으면 디렉터리 수집 공격을 효과적으로 막을 수 없고 간격이 너무 길면 정상적인 메일 흐름을 방해할 수 있으므로 타피팅 간격을 변경할 때는 조금씩 늘리면서 결과를 확인하는 방식을 사용하는 것이 좋습니다. 예를 들어 5초가 효과가 없으면 간격을 10초로 변경해봅니다.

맨 위로 이동

## 여러 네임스페이스

받는 사람 필터 에이전트는 신뢰할 수 있는 도메인에 대해서만 받는 사람 조회를 수행합니다. 조직이 내부 릴레이 또는 외부 릴레이 도메인으로 구성된 다른 도메인 대신 메시지를 수락하거나 전달하는 경우 받는 사람 필터 에이전트는 해당 도메인의 받는 사람에 대해서는 받는 사람 조회를 수행하지 않습니다. 하지만 받는 사람이 차단된 받는 사람 목록에 지정되어 있는 경우에는 받는 사람 필터 에이전트에 의해 차단됩니다.

Edge 전송 서버에서 로컬로 허용 도메인을 구성할 수도 있습니다. 도메인이 내부 릴레이 또는 외부 릴레이 도메인으로 구성된 경우 Edge 전송 서버의 받는 사람 필터 에이전트 역시 해당 도메인의 받는 사람에 대해 받는 사람 조회를 수행하지 않습니다.

맨 위로 이동
