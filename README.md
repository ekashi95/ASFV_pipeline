<details>
<summary><img height="20" src="icons/ru.svg" alt="RU"/> ASFV_pipeline (версия с низким потреблением памяти): инструкция на русском языке</summary>

## ASFV_pipeline (версия с низким потреблением памяти)
В пайплайне из оригинального репозитория используется утилита ``bwa-mem2``, очень требовательная к объёму оперативной памяти. Здесь предлагается не самое изящное, но рабочее решение по замене в наиболее ресурсоёмких операциях ``bwa-mem2`` на ``bwa``. 

Если пайплайн из оригинального репозитория на вашем компьютере падает с ошибкой из-за нехватки оперативной памяти на этапе индексирования генома *Sus scrofa* — возможно, этот форк решит вашу проблему. 

Контейнер Docker, используемый в оригинальном репозитории, остался неизменным. Перед запуском пайплайна дополнительно загружается утилита ``bwa``, а также создаётся скрипт-перехватчик команд ``bwa-mem2``, который будет запускать ``bwa`` вместо ``bwa-mem2``. 

### Инструкция (Ubuntu)

1. Загрузите папку `pipeline` из репозитория. Заполните поля в CSV-файле `MetadataNew.csv`. Этот файл можно настроить для выполнения одного или нескольких последовательных запусков. Для каждого образца заполните строку в соответствии со следующими инструкциями:

	1. Столбцы ``L1R1``, ``L1R2``, ``L2R1``, ``L2R2``, ``L3R1``, ``L3R2``, ``L4R1``, ``L4R2`` относятся к чтениям Illumina: ``L<номер дорожки>R<номер рида>``. Укажите в этих столбцах пути к файлам с чтениями, например:

		| L1R1 | L1R2 |
		| --- | --- | 
		| ``Input/SampleAA_R1_001.fastq.gz`` | ``Input/SampleAA_R2_001.fastq.gz`` | 

		В дорожках ``L1R1`` и ``L1R2`` обязательно должны быть риды, остальные столбцы могут оставаться пустыми, если не были сгенерированы соответствующие файлы данных.

	2. Если у вас имеются данные секвенирования Nanopore, то укажите путь к соответствующей папке в столбце `MinionDirectory`. Обязательно поставьте `/` в конце. Например:

		| MinionDirectory | 
		| --- | 
		| `Minion/` | 

		Если данных Nanopore нет, оставьте этот столбец пустым. От наличия пути к этой директории в файле `MetadataNew.csv` зависит, по какому алгоритму будет работать пайплайн. 

	3. Остальные столбцы менее важны, они могут содержать любую строку. Данные, указанные в этих столбцах, будут использованы для генерации файла в формате GenBank.

	4. Вы можете указать имя проекта в столбце `Project_ID (internal)`. В этом случае заданное имя будет использоваться в именах выходных файлов. 


3. Установите Docker Desktop по инструкции с официального сайта (https://docs.docker.com/desktop/setup/install/linux/ubuntu/) и запустите его. (Вместо Docker Desktop можно установить и Docker Engine, тогда он запустится из командной строки.)

4. Загрузите образ Docker-контейнера: 
```
docker pull garadock/asfv_denovo_assembly_pipeline:v06
```

Запустите контейнер:
```
docker run -it -v "$HOME":"$HOME" -w "$PWD" garadock/asfv_denovo_assembly_pipeline:v06 /bin/bash
```

4. Зайдите в папку с файлами пайплайна и запустите скрипт ``1.sh``:
```
chmod 777 1.sh
./1.sh
```

Если загрузка ``bwa`` падает с ошибкой из-за невозможности подключиться к определённым серверам, попробуйте раскомментировать первые две строки в файле ``1.sh`` и заново запустить этот скрипт: тогда загрузка будет осуществляться с зеркала "Яндекса". 

5. Запустите пайплайн:
```
python3 ASFV_Pipeline.py
```

Будьте готовы, что работа пайплайна займёт несколько часов: индексация генома *Sus scrofa* требует много времени. 

Необходимые файлы для запуска будут загружены в ближайшее время. 

Обратите также внимание на README оригинального репозитория: https://github.com/Global-ASFV-Research-Alliance/ASFV_Pipeline
</details>

<details>
<summary><img height="20" src="icons/en.svg" alt="EN"/> ASFV_pipeline (low-memory fork): English Instructions</summary>

## ASFV_pipeline (low-memory fork)
The pipeline from the original repository uses the bwa-mem2 utility, which is very memory-intensive. This isn't the most elegant, but it works by replacing bwa-mem2 with bwa for the most resource-intensive operations.

The necessary files for running pipeline will be uploaded shortly.

Below is the README from the original repository.
</details>


#### Example of Filling Out MetaDataNew.csv
This is an example of a properly filled out MetaDataNew.csv with two rows. You can click on the image to zoom in. Notice that forward slashes / are used instead of backslashes \ .
![image](https://github.com/Global-ASFV-Research-Alliance/ASFV_Pipeline/assets/103464896/247b41a9-e4d5-4795-972f-4172f8d9c991)
