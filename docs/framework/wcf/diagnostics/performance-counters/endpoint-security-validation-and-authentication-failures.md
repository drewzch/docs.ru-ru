---
title: 'Конечная точка: Сбои при проверке безопасности и проверке подлинности'
ms.date: 03/30/2017
ms.assetid: 5bad60aa-6084-4c7b-aefd-9b581f04382e
ms.openlocfilehash: 9e0192ea600bb52abd555f2f83cfe8e96d3fe203
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2019
ms.locfileid: "64619344"
---
# <a name="endpoint-security-validation-and-authentication-failures"></a>Конечная точка: Сбои при проверке безопасности и проверке подлинности
Имя счетчика: Сбои при проверке безопасности и проверке подлинности  
  
## <a name="description"></a>Описание  
 Значение этого счетчика увеличивается всякий раз, когда сообщение отклоняется из-за проблемы безопасности, не относящейся к счетчику "Security Calls Not Authorized". К таким проблемам относятся следующие.  
  
- Невозможно прочесть в этом сообщении маркер клиента.  
  
- Токен клиента не прошел проверку подлинности (неправильный пароль).  
  
- Сбой при проверке подписи (сообщение было изменено).  
  
- Сообщение является копией предыдущего, что может свидетельствовать об атаке воспроизведения.  
  
- Произошел сбой при расшифровке.  
  
- В сообщении отсутствуют некоторые обязательные элементы (отметка времени или зашифрованный блок данных).  
  
- Во время подтверждения TLSNEGO/SPNEGO произошли ошибки.
