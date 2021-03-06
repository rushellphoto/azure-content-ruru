<properties
	pageTitle="Создание веб-приложения PHP-MySQL в службе приложений Azure и развертывание с помощью Git"
	description="В учебнике описывается, как создать веб-приложение PHP, которое хранит данные в MySQL, и развернуть его в Azure с помощью Git."
	services="app-service\web"
	documentationCenter="php"
	authors="rmcmurray"
	manager="wpickett"
	editor=""
	tags="mysql"/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="PHP"
	ms.topic="article"
	ms.date="06/24/2016"
	ms.author="robmcm"/>

# Создание веб-приложения PHP-MySQL в службе приложений Azure и развертывание с помощью Git

В этом учебнике описывается, как создать веб-приложение PHP-MySQL и развернуть его в [службе приложений](http://go.microsoft.com/fwlink/?LinkId=529714) с помощью Git. Вы будете использовать [PHP][install-php], средство командной строки MySQL (которое входит в состав [MySQL][install-mysql]) и [Git][install-git], установленный на компьютере. Инструкции, приведенные в этом учебнике, применимы к любой операционной системе, включая Windows, Mac и Linux. По завершении работы с этим учебником у вас будет работающее в Azure веб-приложение PHP-MySQL.

Вы узнаете:

* Как создать веб-приложение и базу данных MySQL с помощью [портала Azure][management-portal]. Поскольку модуль PHP включен в [веб-приложениях службы приложений](http://go.microsoft.com/fwlink/?LinkId=529714) по умолчанию, никаких дополнительных действий для выполнения кода PHP не требуется.
* Как опубликовать и повторно опубликовать свое приложение в Azure с помощью Git.
* Как включить расширение Composer для автоматизации задач Composer при каждой операции `git push`.

Руководствуясь этим учебником, вы создадите на языке PHP простое веб-приложение для регистрации. Это приложение будет размещено в веб-приложениях. Снимок экрана завершенного приложения приведен ниже:

![Веб-сайт Azure на PHP][running-app]

## Настройка среды разработки

При работе с этим руководством предполагается, что на компьютере установлены [PHP][install-php], интерфейс командной стройки MySQL (входит в состав [MySQL][install-mysql]) и [Git][install-git].

<a id="create-web-site-and-set-up-git"></a>
## Создание веб-приложения и настройка публикации через Git

Чтобы создать веб-приложение и базу данных MySQL, выполните следующие действия.

1. Войдите на [портал Azure][management-portal].
2. Щелкните значок **Создать**.

3. Щелкните **Показать все** рядом с **Marketplace**.

4. Щелкните **Интернет + мобильные устройства**, а затем **Веб-приложение + MySQL**. Затем щелкните **Создать**.

4. Укажите действительное имя группы ресурсов.

    ![Задание имени группы ресурсов][resource-group]

5. Введите значения для нового веб-приложения.

    ![Создание веб-приложения][new-web-app]

6. Введите значения для своей новой базы данных и примите условия.

	![Создание новой базы данных MySQL][new-mysql-db]

7. Когда веб-приложение будет создано, вы увидите колонку нового веб-приложения.

7. В колонке **Параметры** щелкните **Непрерывное развертывание**, а затем _Настроить необходимые параметры_.

	![Настройка публикации Git][setup-publishing]

8. Выберите для исходного кода **Локальный репозиторий Git**.

    ![Настройка репозитория Git][setup-repository]


9. Чтобы включить публикацию Git, необходимо предоставить имя пользователя и пароль. Запишите создаваемые имя пользователя и пароль. (Если ранее уже был настроен репозиторий Git, этот шаг будет пропущен.)

	![Создание учетных данных для публикации][credentials]


## Получение информации об удаленном подключении к MySQL

Чтобы подключиться к базе данных MySQL, которая работает в веб-приложениях, вам потребуются сведения о подключении. Чтобы получить сведения о подключении к MySQL, выполните следующие действия.

1. В группе ресурсов щелкните базу данных.

	![Выбор базы данных][select-database]

2. В колонке **Параметры** базы данных выберите пункт **Свойства**.

    ![Выбор свойств][select-properties]

2. Запишите значения `Database`, `Host`, `User Id` и `Password`.

    ![Запись свойств][note-properties]

## Локальное создание и тестирование приложения

Теперь, когда у вас есть созданное веб-приложение, вы можете разработать свое приложение локально и после тестирования развернуть его.

Приложение регистрации представляет собой простое приложение PHP, которое позволяет регистрироваться на мероприятие, предоставляя свое имя и адрес электронной почты. Сведения о ранее зарегистрировавшихся пользователях отображаются в таблице. Сведения о регистрации хранятся в базе данных MySQL. Приложение состоит из одного файла (код для копирования и вставки приведен ниже):

* **index.php**: вывод формы для регистрации и таблицы, содержащей сведения о зарегистрировавшихся пользователях.

Чтобы построить и запустить приложение локально, выполните следующие действия. Обратите внимание: предполагается, что на локальном компьютере установлены PHP и средство командной строки MySQL (входит в комплект поставки MySQL), а также включено [расширение PDO для MySQL][pdo-mysql]

1. Подключитесь к удаленному серверу MySQL с использованием раннее полученных значений `Data Source`, `User Id`, `Password` и `Database`.

		mysql -h{Data Source] -u[User Id] -p[Password] -D[Database]

2. Появится командная строка MySQL:

		mysql>

3. Вставьте команду `CREATE TABLE`, чтобы создать в базе данных таблицу `registration_tbl`.

		CREATE TABLE registration_tbl(id INT NOT NULL AUTO_INCREMENT, PRIMARY KEY(id), name VARCHAR(30), email VARCHAR(30), date DATE);

4. В корневой папке локального приложения создайте файл **index.php**.

5. Откройте файл **index.php** в текстовом редакторе или в интегрированной среде разработки и добавьте следующий код, после чего внесите необходимые изменения, обозначенные комментарием `//TODO:`.


		<html>
		<head>
		<Title>Registration Form</Title>
		<style type="text/css">
			body { background-color: #fff; border-top: solid 10px #000;
			    color: #333; font-size: .85em; margin: 20; padding: 20;
			    font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
			}
			h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
			h1 { font-size: 2em; }
			h2 { font-size: 1.75em; }
			h3 { font-size: 1.2em; }
			table { margin-top: 0.75em; }
			th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
			td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
		</style>
		</head>
		<body>
		<h1>Register here!</h1>
		<p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
		<form method="post" action="index.php" enctype="multipart/form-data" >
		      Name  <input type="text" name="name" id="name"/></br>
		      Email <input type="text" name="email" id="email"/></br>
		      <input type="submit" name="submit" value="Submit" />
		</form>
		<?php
			// DB connection info
			//TODO: Update the values for $host, $user, $pwd, and $db
			//using the values you retrieved earlier from the Azure Portal.
			$host = "value of Data Source";
			$user = "value of User Id";
			$pwd = "value of Password";
			$db = "value of Database";
			// Connect to database.
			try {
				$conn = new PDO( "mysql:host=$host;dbname=$db", $user, $pwd);
				$conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
			}
			catch(Exception $e){
				die(var_dump($e));
			}
			// Insert registration info
			if(!empty($_POST)) {
			try {
				$name = $_POST['name'];
				$email = $_POST['email'];
				$date = date("Y-m-d");
				// Insert data
				$sql_insert = "INSERT INTO registration_tbl (name, email, date)
						   VALUES (?,?,?)";
				$stmt = $conn->prepare($sql_insert);
				$stmt->bindValue(1, $name);
				$stmt->bindValue(2, $email);
				$stmt->bindValue(3, $date);
				$stmt->execute();
			}
			catch(Exception $e) {
				die(var_dump($e));
			}
			echo "<h3>Your're registered!</h3>";
			}
			// Retrieve data
			$sql_select = "SELECT * FROM registration_tbl";
			$stmt = $conn->query($sql_select);
			$registrants = $stmt->fetchAll();
			if(count($registrants) > 0) {
				echo "<h2>People who are registered:</h2>";
				echo "<table>";
				echo "<tr><th>Name</th>";
				echo "<th>Email</th>";
				echo "<th>Date</th></tr>";
				foreach($registrants as $registrant) {
					echo "<tr><td>".$registrant['name']."</td>";
					echo "<td>".$registrant['email']."</td>";
					echo "<td>".$registrant['date']."</td></tr>";
		    	}
		 		echo "</table>";
			} else {
				echo "<h3>No one is currently registered.</h3>";
			}
		?>
		</body>
		</html>

4.  В терминале перейдите в папку приложения и введите следующую команду:

		php -S localhost:8000

Теперь можно перейти к **http://localhost:8000/** для тестирования приложения.


## Публикация приложения

Протестировав приложение локально, вы можете опубликовать его в веб-приложениях с помощью Git. Необходимо инициализировать локальный репозиторий Git и опубликовать свое приложение.

> [AZURE.NOTE]
Для этого на портале Azure следует выполнить действия, которые описаны выше в конце раздела "Создание веб-приложения и настройка публикации через Git".

1. Если вы забыли или потеряли записанный URL-адрес удаленного репозитория Git, откройте на портале Azure свойства своего веб-приложения.

1. Откройте GitBash (или терминал, если Git включен в переменную `PATH`), измените каталог на корневой каталог приложения и выполните следующие команды:

		git init
		git add .
		git commit -m "initial commit"
		git remote add azure [URL for remote repository]
		git push azure master

	Появится запрос на ввод ранее заданного пароля.

	![Исходная отправка в Azure через Git][git-initial-push]

2. Перейдите к **http://[site имя].azurewebsites.net/index.php**, чтобы начать использование приложения (эта информация будет сохранена на панели мониторинга учетной записи).

	![Веб-сайт Azure на PHP][running-app]

Опубликовав приложение, вы сможете вносить в него изменения и публиковать их с помощью Git.

## Публикация изменений в приложении

Чтобы опубликовать изменения в приложении, выполните следующие действия.

1. Измените приложение локально.
2. Откройте GitBash (или терминал, если Git указан в переменной `PATH`), измените каталоги на корневой каталог приложения и выполните следующие команды:

		git add .
		git commit -m "comment describing changes"
		git push azure master

	Появится запрос на ввод ранее заданного пароля.

	![Отправка изменений сайта в Azure через Git][git-change-push]

3. Перейдите к **http://[site имя].azurewebsites.net/index.php**, чтобы просмотреть приложение и все внесенные изменения.

	![Веб-сайт Azure на PHP][running-app]

>[AZURE.NOTE] Чтобы приступить к работе со службой приложений Azure до создания учетной записи Azure, перейдите к разделу [Пробное использование службы приложений](http://go.microsoft.com/fwlink/?LinkId=523751), где вы можете быстро создать кратковременное веб-приложение начального уровня в службе приложений. Никаких кредитных карт и обязательств.

<a name="composer"></a>
## Включение расширения Composer для автоматизации задач Composer

По умолчанию процесс развертывания git в службе приложений не выполняет никаких действий с файлом composer.json, если он есть в проект PHP. Чтобы включить обработку composer.json во время операции `git push`, активируйте расширение Composer.

1. На [портале Azure][management-portal] в колонке веб-приложения PHP щелкните **Инструменты** > **Расширения**.

    ![Параметры расширения Composer][composer-extension-settings]

2. Щелкните **Добавить**, а затем — **Composer**.

    ![Добавление расширения Composer][composer-extension-add]
    
3. Нажмите кнопку **ОК**, чтобы принять условия использования. Нажмите кнопку **ОК** еще раз, чтобы добавить расширение.

    В колонке **Установленные расширения** появится расширение Composer. ![Просмотр расширения Composer][composer-extension-view]
    
4. Теперь выполните операции `git add`, `git commit` и `git push`, как в предыдущем разделе. Вы увидите, что расширение Composer устанавливает зависимости, определенные в файле composer.json.

    ![Добавленное расширение Composer][composer-extension-success]

## Дальнейшие действия

Дополнительную информацию можно найти в [Центре разработчика PHP](/develop/php/).

<!-- URL List -->

[install-php]: http://www.php.net/manual/en/install.php
[install-SQLExpress]: http://www.microsoft.com/download/details.aspx?id=29062
[install-Drivers]: http://www.microsoft.com/download/details.aspx?id=20098
[install-git]: http://git-scm.com/
[install-mysql]: http://dev.mysql.com/downloads/mysql/
[pdo-mysql]: http://www.php.net/manual/en/ref.pdo-mysql.php
[management-portal]: https://portal.azure.com
[sql-database-editions]: http://msdn.microsoft.com/library/windowsazure/ee621788.aspx

<!-- IMG List -->

[running-app]: ./media/web-sites-php-mysql-deploy-use-git/running_app_2.png
[new-website]: ./media/web-sites-php-mysql-deploy-use-git/new_website2.png
[custom-create]: ./media/web-sites-php-mysql-deploy-use-git/create_web_mysql.png
[new-mysql-db]: ./media/web-sites-php-mysql-deploy-use-git/create_db.png
[go-to-webapp]: ./media/web-sites-php-mysql-deploy-use-git/select_webapp.png
[setup-git-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_git_publishing.png
[credentials]: ./media/web-sites-php-mysql-deploy-use-git/save_credentials.png
[resource-group]: ./media/web-sites-php-mysql-deploy-use-git/set_group.png
[new-web-app]: ./media/web-sites-php-mysql-deploy-use-git/create_wa.png
[setup-publishing]: ./media/web-sites-php-mysql-deploy-use-git/setup_deploy.png
[setup-repository]: ./media/web-sites-php-mysql-deploy-use-git/select_local_git.png
[select-database]: ./media/web-sites-php-mysql-deploy-use-git/select_database.png
[select-properties]: ./media/web-sites-php-mysql-deploy-use-git/select_properties.png
[note-properties]: ./media/web-sites-php-mysql-deploy-use-git/note-properties.png

[git-instructions]: ./media/web-sites-php-mysql-deploy-use-git/git-instructions.png
[git-change-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-change-push.png
[git-initial-push]: ./media/web-sites-php-mysql-deploy-use-git/php-git-initial-push.png

[composer-extension-settings]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-settings.png
[composer-extension-add]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-add.png
[composer-extension-view]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-view.png
[composer-extension-success]: ./media/web-sites-php-mysql-deploy-use-git/composer-extension-success.png

<!---HONumber=AcomDC_0629_2016-->