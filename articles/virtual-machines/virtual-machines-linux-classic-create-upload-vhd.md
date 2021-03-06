<properties
	pageTitle="Создание и передача виртуального жесткого диска Linux | Microsoft Azure"
	description="Создание и передача виртуального жесткого диска Azure, содержащего операционную систему Linux, с использованием классической модели развертывания."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/14/2016"
	ms.author="iainfou"/>

# Создание и передача виртуального жесткого диска с операционной системой Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

В этой статье показано, как создать и передать виртуальный жесткий диск (VHD-файл), чтобы использовать его в качестве образа для создания виртуальных машин в Azure. Вы узнаете, как подготовить операционную систему, чтобы использовать ее в качестве образа для создания нескольких виртуальных машин.

**Важно!** Соглашение об уровне обслуживания платформы Azure применяется к виртуальным машинам, работающим под управлением ОС Linux, только если один из рекомендуемых дистрибутивов используется вместе со сведениями о конфигурации, указанными в статье «Поддерживаемые версии» раздела [Linux с рекомендуемыми дистрибутивами Azure](virtual-machines-linux-endorsed-distros.md). Все дистрибутивы Linux в коллекции образов Azure — это рекомендуемые дистрибутивы, уже имеющие необходимую конфигурацию.


## Предварительные требования
В данной статье предполагается, что у вас есть следующие элементы:

- **Операционная система Linux, установленная в VHD-файле**. Вы установили [рекомендуемый для Azure дистрибутив Linux](virtual-machines-linux-endorsed-distros.md) (или см. [информацию о нерекомендованных дистрибутивах](virtual-machines-linux-create-upload-generic.md)) на виртуальный диск в формате VHD. Существует несколько инструментов для создания VHD-файлов. Например, для создания VHD-файла и установки операционной системы можно использовать решения для виртуализации, такие как Hyper-V. Инструкции см. в разделе [Установка роли Hyper-V и настройка виртуальной машины](http://technet.microsoft.com/library/hh846766.aspx).

	> [AZURE.NOTE] Более новый формат VHDX не поддерживается в Azure. Можно преобразовать VHDX в формат VHD с помощью диспетчера Hyper-V или командлета `Convert-VHD`. Кроме того, Azure не поддерживает отправку динамических дисков VHD, поэтому перед отправкой необходимо преобразовать такие диски в статические диски VHD. Для преобразования динамических дисков можно использовать [служебные программы Azure VHD для GO](https://github.com/Microsoft/azure-vhd-utils-for-go).

- **Интерфейс командной строки Azure**. Установите последнюю версию [интерфейса командной строки Azure](../virtual-machines-command-line-tools.md) для отправки дисков VHD.

<a id="prepimage"> </a>
## Шаг 1. Подготовка образа для передачи

Microsoft Azure поддерживает различные дистрибутивы Linux (см. раздел [Рекомендованные дистрибутивы](virtual-machines-linux-endorsed-distros.md)). В следующих статьях описывается подготовка различных дистрибутивов Linux, которые поддерживаются в Azure. После выполнения этапов, описанных в приведенных ниже руководствах, вернитесь сюда. У вас уже должен быть VHD-файл для передачи в Azure.

- **[Дистрибутивы на основе CentOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES и openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Прочее — нерекомендованные дистрибутивы](virtual-machines-linux-create-upload-generic.md)**

Дополнительные общие советы по подготовке образов Linux для Azure см. в разделе **[Общие замечания по установке Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)**.


<a id="connect"> </a>
## Шаг 2. Подготовка подключения к Azure

Убедитесь, что вы используете интерфейс командной строки Azure в классической модели развертывания (`azure config mode asm`), а затем войдите в свою учетную запись:

```
azure login
```


<a id="upload"> </a>
## Шаг 3. Передача образа в Azure

Потребуется учетная запись хранения для отправки VHD-файла. На этом шаге можно выбрать существующую учетную запись или [создать новую](../storage/storage-create-storage-account.md).

Для передачи образа выполните следующую команду в командной строке Azure:

		azure vm image create <ImageName> --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> --os Linux <PathToVHDFile>

В предыдущем примере:

- **BlobStorageURL** — URL-адрес для учетной записи хранения, которую вы планируете использовать.
- **YourImagesFolder** — контейнер внутри хранилища BLOB-объектов, где будут храниться образы.
- **VHDName** — метка, которая отображается на портале для идентификации виртуального жесткого диска.
- **PathToVHDFile** — полный путь и имя VHD-файла на виртуальной машине.

Дополнительные сведения см. в [справочнике по командам Azure CLI для управления службами Azure](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload

<!---HONumber=AcomDC_0629_2016-->