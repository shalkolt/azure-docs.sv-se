---
title: Kopiera data från Amazon Simple Storage Service med Azure Data Factory | Microsoft Docs
description: Läs mer om hur du kopierar data från Amazon Simple Storage Service (S3) till datalager för mottagare som stöds med hjälp av Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: jingwang
ms.openlocfilehash: bdbf0b74b6e851e0dd84ff5d9aafb84d878d8ea2
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542082"
---
# <a name="copy-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Kopiera data från Amazon Simple Storage Service med Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1](v1/data-factory-amazon-simple-storage-service-connector.md)
> * [Aktuell version](connector-amazon-simple-storage-service.md)

Den här artikeln beskrivs hur du använder Kopieringsaktivitet i Azure Data Factory för att kopiera data från Amazon S3. Den bygger på den [översikt över Kopieringsaktivitet](copy-activity-overview.md) artikel som ger en allmän översikt över Kopieringsaktivitet.

## <a name="supported-capabilities"></a>Funktioner som stöds

Du kan kopiera data Amazon S3 till alla datalager för mottagare som stöds. En lista över datalager som stöds som källor och mottagare av Kopieringsaktivitet finns i den [datalager som stöds](copy-activity-overview.md#supported-data-stores-and-formats) tabell.

Mer specifikt den här Amazon S3-anslutningsappen stöder kopiera filer som-är eller parsningsfilerna med den [stöds filformat och komprimering codec](supported-file-formats-and-compression-codecs.md).

## <a name="required-permissions"></a>Nödvändiga behörigheter

För att kopiera data från Amazon S3, kontrollera att du har beviljats följande behörigheter:

- `s3:GetObject` och `s3:GetObjectVersion` för Amazon S3-objektet.
- `s3:ListBucket` eller `s3:GetBucketLocation` för Amazon S3-Bucket åtgärder. 

>[!NOTE]
>När du använder Data Factory-Gränssnittet för redigering, `s3:ListAllMyBuckets` behörighet krävs också för, till exempel Testa anslutning och bläddra/navigera filsökvägar. Om du inte vill bevilja den här behörigheten kan du hoppa över Testa anslutning i sidan för att skapa den länkade tjänsten och speicify sökvägen direkt i inställningarna för datauppsättningen.

Mer information om en fullständig lista över Amazon S3-behörigheter finns i [att ange behörigheter i en princip](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Komma igång

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)] 

Följande avsnitt innehåller information om egenskaper som används för att definiera Data Factory-entiteter som är specifika för Amazon S3.

## <a name="linked-service-properties"></a>Länkade tjänstegenskaper

Följande egenskaper har stöd för Amazon S3 länkade tjänsten:

| Egenskap  | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ | Type-egenskapen måste anges till **AmazonS3**. | Ja |
| accessKeyId | ID för den hemliga åtkomstnyckeln. |Ja |
| secretAccessKey | Den hemliga åtkomstnyckeln själva. Markera det här fältet som en SecureString ska lagras på ett säkert sätt i Data Factory, eller [refererar till en hemlighet som lagras i Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | Den [Integration Runtime](concepts-integration-runtime.md) som används för att ansluta till datalagret. Du kan använda Azure Integration Runtime eller lokal Integration Runtime (om ditt datalager finns i privat nätverk). Om den inte anges används standard Azure Integration Runtime. |Nej |

>[!NOTE]
>Den här anslutningen kräver åtkomstnycklarna för IAM-konto för att kopiera data från Amazon S3. [Tillfällig autentiseringsuppgift](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) stöds inte.
>

Här är ett exempel:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AmazonS3",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": {
                    "type": "SecureString",
                    "value": "<secret access key>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Egenskaper för datamängd

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns i artikeln datauppsättningar. Det här avsnittet innehåller en lista över egenskaper som stöds av Amazon S3-datamängd.

För att kopiera data från Amazon S3, ange typegenskapen på datauppsättningen till **AmazonS3Object**. Följande egenskaper stöds:

| Egenskap  | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ | Type-egenskapen för datauppsättningen måste anges till: **AmazonS3Object** |Ja |
| bucketName | S3-Bucketnamn. Jokerteckenfilter stöds inte. |Ja |
| key | Den **namn eller jokertecken-filtret** för S3 Objektnyckel under en angiven bucket. Gäller endast när egenskapen ”prefixet” inte har angetts. <br/><br/>Jokerteckenfiltret har endast stöd för Namndelen för filen men inte mappdelen. Tillåtna jokertecken är: `*` (matchar noll eller flera tecken) och `?` (matchar noll eller valfritt tecken).<br/>– Exempel 1: `"key": "rootfolder/subfolder/*.csv"`<br/>– Exempel 2: `"key": "rootfolder/subfolder/???20180427.txt"`<br/>Använd `^` att undvika om din faktiska filnamnet har jokertecken eller den här escape-tecken i. |Nej |
| prefix | Prefix för Objektnyckel S3. Objekt vars nycklar som börjar med prefixet är markerade. Gäller endast när egenskapen ”nyckeln” inte har angetts. |Nej |
| version | Versionen av S3-objektet, om S3 versionshantering är aktiverad. |Nej |
| Format | Om du vill **kopiera filer som – är** hoppa över avsnittet format i både inkommande och utgående datamängd definitioner mellan filbaserade (binär kopia).<br/><br/>Om du vill parsa eller generera filer med ett visst format format för följande filtyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Ange den **typ** egenskapen under format till ett av dessa värden. Mer information finns i [textformat](supported-file-formats-and-compression-codecs.md#text-format), [Json-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-formatet](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-Format](supported-file-formats-and-compression-codecs.md#orc-format), och [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format) avsnitt. |Nej (endast för binär kopia scenario) |
| Komprimering | Ange typ och komprimeringsnivå för data. Mer information finns i [stöds filformat och komprimering codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.<br/>Nivåer som stöds är: **Optimal** och **snabbast**. |Nej |

>[!TIP]
>Kopiera alla filer i en mapp genom att ange **bucketName** för bucket och **prefix** för mappdelen.<br>Om du vill kopiera en enstaka fil med ett givet namn, ange **bucketName** för bucket och **nyckel** för en del plus filen mappnamn.<br>Om du vill kopiera en delmängd av filerna under en mapp, anger **bucketName** för bucket och **nyckel** för mappen del plus jokertecken-filtret.

**Exempel: med prefix**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3Object",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "prefix": "testFolder/test",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

**Exempel: med hjälp av nyckeln och version (valfritt)**

```json
{
    "name": "AmazonS3Dataset",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": {
            "referenceName": "<Amazon S3 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "bucketName": "testbucket",
            "key": "testFolder/testfile.csv.gz",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopiera egenskaper för aktivitet

En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera aktiviteter finns i den [Pipelines](concepts-pipelines-activities.md) artikeln. Det här avsnittet innehåller en lista över egenskaper som stöds av Amazon S3-källan.

### <a name="amazon-s3-as-source"></a>Amazon S3 som källa

För att kopiera data från Amazon S3, ange typ av datakälla i kopieringsaktiviteten till **FileSystemSource** (som innehåller Amazon S3). Följande egenskaper stöds i kopieringsaktiviteten **källa** avsnittet:

| Egenskap  | Beskrivning | Krävs |
|:--- |:--- |:--- |
| typ | Type-egenskapen för aktiviteten kopieringskälla måste anges till: **FileSystemSource** |Ja |
| rekursiv | Anger om data läses rekursivt från undermappar eller endast från den angivna mappen. Obs när rekursiv har angetts till true och mottagare är filbaserade store, tom mapp/underanvändningsfall-folder kan inte kopieras/skapas vid mottagare.<br/>Tillåtna värden är: **SANT** (standard), **FALSKT** | Nej |

**Exempel:**

```json
"activities":[
    {
        "name": "CopyFromAmazonS3",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Amazon S3 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="next-steps"></a>Nästa steg
En lista över datalager som stöds som källor och mottagare av kopieringsaktiviteten i Azure Data Factory finns i [datalager som stöds](copy-activity-overview.md##supported-data-stores-and-formats).
