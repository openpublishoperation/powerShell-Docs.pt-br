---
title: Trabalhando com impressoras
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 4f29ead3-f83b-4706-ac3e-f2154ff38dc5
translationtype: Human Translation
ms.sourcegitcommit: 03ac4b90d299b316194f1fa932e7dbf62d4b1c8e
ms.openlocfilehash: 27d3d11b71b95cd79817449cf8bdb1a0a26936bd

---

# Trabalhando com impressoras
Você pode usar o Windows PowerShell para gerenciar impressoras usando o WMI e o objeto COM WScript.Network do WSH. Usaremos uma combinação das duas ferramentas para demonstrar as tarefas específicas.

### Listar conexões de impressora
A maneira mais simples de listar as impressoras instaladas em um computador é usar a classe **Win32\_Printer** do WMI:

```
Get-WmiObject -Class Win32_Printer -ComputerName
```

Você também pode listar as impressoras usando o objeto COM **WScript.Network** que normalmente é usado nos scripts do WSH:

```
(New-Object -ComObject WScript.Network).EnumPrinterConnections()
```

Como esse comando retorna uma coleção simples de cadeia de caracteres de nomes de portas e nomes de dispositivo de impressora sem qualquer distinção rótulos, por isso não é fácil interpretar.

### Adicionar uma impressora de rede
Para adicionar uma nova impressora de rede, use **WScript.Network**:

```
(New-Object -ComObject WScript.Network).AddWindowsPrinterConnection("\\Printserver01\Xerox5")
```

### Configurar uma impressora padrão
Para usar o WMI para definir a impressora padrão, encontre a impressora na coleção **Win32\_Printer** e invoque o método **SetDefaultPrinter**:

```
(Get-WmiObject -ComputerName . -Class Win32_Printer -Filter "Name='HP LaserJet 5Si'").SetDefaultPrinter()
```

**WScript.Network** é um pouco mais simples de usar, pois ele tem um método **SetDefaultPrinter** que usa apenas o nome da impressora como um argumento:

```
(New-Object -ComObject WScript.Network).SetDefaultPrinter('HP LaserJet 5Si')
```

### Remover a conexão da impressora
Para remover uma conexão de impressora, use o método **WScript.Network RemovePrinterConnection**:

```
(New-Object -ComObject WScript.Network).RemovePrinterConnection("\\Printserver01\Xerox5")
```




<!--HONumber=Jun16_HO4-->


