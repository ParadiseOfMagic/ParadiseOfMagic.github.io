---
layout: post
title: "Using Linux as a MCAST student: Trials and Tribulations"
date:   2026-02-14 13:50 +0100
categories: MCAST
---

At MCAST, I opted to use Linux for my studies. This meant I had to come with
some workarounds, this post can also serve as a pseudo-guide on what software
to use for those like me. At the time of writing, I'm in my second year and
final semester for my advanced diploma so I might miss a few details however,
most of what I did should be here.

## First year, both semesters:

During the first year, I was first dual-booting Windows 10 and Linux so I used
less workarounds. I started with Linux Mint as most beginners to desktop Linux
do. I had some trouble with Cisco Packet Tracer since finding and installing
the .deb took some effort. My lack of experience probably contributed to that.
I did start dabbling in (my now favourite text editor) Neovim, I still Visual
Studio Code for school and my own personal projects. A decision I honestly
regret, considering how terrible my laptop was. Visual Studio Code would often
crash or freeze, making it hard to focus.

## Second year:

### First semester:

Now this is where gets interesting as I was now using Linux(Arch Linux) only on
my new laptop. The first hurdle was C# development, I first started with
virtual machine through Qemu running Visual Studio. I grew some confidence and actually
learned Neovim, I had started with Omnisharp unfortunately that language server
is quite weak and slow.

I eventually used a plug-in by the name of 'rosyln.nvim' together with the
language server Roslyn. This got into tooling such as formatters,linters and
language servers, I grew to appreciate them especially formatters as in one of
my previous projects a game made with LOVE2D the formatting drove me crazy.
Learning Roslyn's code actions too were quite nice too. At the time I did not
know about Visual Studio Code's C# extension though, I find Neovim more fun so
I do not regret it.

I too had to learn how to use Dotnet from the cli but, that was quite pleasant
and easy. Here are some commands for any student reading who is like me or just
wants to use Dotnet from the cli too.

How to create a console project with a name.
```bash
dotnet new console --name <name of project>
```
How to run a project. Path and project argument is optional, you can also have your working directory as the project.
```bash
dotnet run --project <project path>
```
How to create a new solution with a name.
```bash
dotnet new sln --name <name of sln>
```
How to add a project to a solution.
```bash
dotnet sln <path to solution> add <path to project>
```

For Java development, Apache Netbeans is fortunately on Linux. I did start with
Neovim and Jdtls(using the Jdtls plug-in) with newfound knowledge of how to use
Neovim, I went back to Netbeans for GUI development and well because we had to
use it for TCAs and CBAs. I went into Netbeans with some extra confidence than
I had before, probably from my success in Neovim and I began exploring key
binds and configuration for Netbeans(and also Visual Studio later on!). I even
learned some key binds that the lecturers didn't know about such as formatting
in Visual Studio(ctrl+e ctrl+f) and auto-complete(ctrl+space) in Netbeans.

During client side scripting, I could have just used Visual Studio Code like
with C# but, I liked Neovim more and knew how to use the same language servers
that Visual Studio Code uses, I also used Biome for formatting and linting for
Javascript,HTML and CSS.

Unity development had probably the most bumps as the AUR package for Unity was
non-functional for about a month after I started MCAST. I first tried using
Distrobox which where I started to learn about containers, I had plenty of
issues being a newbie to containers so I opted for a buggy Flatpak package
instead and using Neovim as my text editor for Unity. The AUR package was
updated after some time which was much smoother, so many thanks to the
maintainer nobbele.


### Second semester

For the second semester, since XAMPP is an AUR package I opted to set up
Apache,Php and MySql(in my case Mariadb). Since the documentation for doing
this on a MAC(That's what I read as MAC and Linux are both Unix-like) is quite
poor on MCAST's end, I'll try to make this section more of a guide of sorts.

First for Php and Mariadb, I had followed the Arch Wiki. Activating some Php modules.

Uncomment these lines in your "/etc/php/php.ini"
```txt
extension=pdo_mysql
extension=mysqli
```
You should also install the Apache Php module as we will need it later.

For Mariadb, I had to create a new user and install the database. I was a bit
confused due to just how poor the MCAST documentation was but, I figured it.
For anyone like me please just read the Arch Wiki it's a godsend for any Linux
user.

Run this command to install the database. Do this before enabling the Systemd service.
```bash
mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```

Run these series of commands to create an user for the database with all privileges.
```bash
$ Mariadb

#(While in Mariadb)
MariaDB> CREATE DATABASE <database name>;
MariaDB> CREATE USER '<username>'@'localhost' IDENTIFIED BY '<password_here>';
MariaDB> GRANT ALL PRIVILEGES ON mydb.* TO '<username>'@'localhost';
MariaDB> quit
```

Then make this Php script and run it with "php <script_name>.php"

```php
<?php
    $servername = "localhost";
    $username = "<username>";
    $password = "<password of the user you created before>";
    $dbname = "<name of the database you made>";

    $conn = mysqli_connect($servername,$username,$password,$dbname);

    if (!$conn) {
        die("connection failed" . mysqli_connect_error());
    }
    echo "Connected successfully";
?>
```
Now let's set up tooling for Php. For Visual Studio Code you can install the Phptools plug-in, for Neovim we will need to set up a language server ourselves I will be using Phpactor in this example.

Install Phpactor using Mason.
```vim
:MasonInstall phpactor
```
Then set it up using the native LSP client. Here is my configuration, you will also need to enable it.
```lua
vim.lsp.enable("phpactor")

vim.lsp.config("phpactor", {
	cmd = { "phpactor", "language-server" },
	filetypes = { "php" },
	root_markers = { ".git", "composer.json", ".phpactor.json", ".phpactor.yml" },
	workspace_required = true,
	init_options = {
		["language_server_phpstan.enabled"] = false,
		["language_server_psalm.enabled"] = false,
	},
})
```
Then for Phpactor, since it needs a workspace just place a .phpactor.json to
allow the server to connect. For a formatter, I use Laravel Pint so just put
that into your Conform configuration like so.
```lua
php = { "pint" },
```

Now it is time for Apache. There is no guide whatsoever from MCAST's end, not that it would likely be any good.

First you want to enable Php support.

Comment out this line.
```txt
#LoadModule mpm_event_module modules/mod_mpm_event.so
```
Uncomment this line.
```txt
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
```

Add this at the end of the "modules" list.
```txt
LoadModule php_module modules/libphp.so
AddHandler php-script .php
```
Add this at the end of the "include" list.
```txt
Include conf/extra/php_module.conf
```

Now we want to set up an user directory as by default Apache looks inside a directory that requires root to access(highly inconvenient).

Change the document root and the directory below it to a directory inside your home.
```txt
DocumentRoot "<directory path>"
<Directory "<same directory path>">
```
And change "require all denied" to "require all granted".

Now go to the directory where you have set your document root to and use UCL to allow the "http" user to access it.
```bash
# Grant execute permissions to http for your home
$ setfacl -m u:http:x ~
# Grant execute and read permissions to http to your DocumentRoot
$ setfacl -R -m u:http:rx ~/public_html
```

I would recommend creating a file named index.html and just make it a simple HTML page to check if Apache works. If you are using the Systemd service you will also need to add this option to the service's hardening configuration(/etc/systemd/system/httpd.service.d/hardening.conf)

```txt
ProtectHome=no
```

Now start Apache either through "apachectl -k start" or the Systemd service and
see if it works. If it does not please read the [official Apache
documentation](https://httpd.apache.org/docs/trunk/mod/directives.html) and the
Arch Wiki.

Now we need to set up Phpmyadmin, the MCAST documentation is once again poor
for this(mentions wrong file). Just install Phpmyadmin and find its root
directory using your package manager and create a soft link to it inside your DocumentRoot like so.

```bash
ln -s /usr/share/webapps/phpMyAdmin <root document>
```

Then navigate to "phpmyadmin/index.php" to access the login page, then login as the user you created before for Mariadb.

Well that was quite long was it not? Thankfully the set up for relation databases is far simpler.

All I had to do was create a Docker container for Microsoft SQL Server, run it and then connect to it through DBeaver.

Here is my compose file.
```yaml
services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: sqlserver
    ports:
      - "1433:1433"
    environment:
      SA_PASSWORD: "<Your password here>"
      ACCEPT_EULA: "Y"
      MSSQL_PID: "Express"
    volumes:
      - sql_db_1:/var/opt/mssql
volumes:
  sql_db_1:
```

I tried doing this with rootless Docker but, had some issues and just went for rootful Docker instead.



This is by far my longest blog post to date, far longer than my previous ones. So I hope it was a good read.

Software mentioned in no particular order:
- [Docker](https://docs.docker.com/get-started/docker-overview/)
- [Visual Studio](https://visualstudio.microsoft.com/)
- [Qemu](https://www.qemu.org/)
- [Apache HTTP server](https://httpd.apache.org/)
- [Php](https://www.php.net/)
- [Arch Linux](https://archlinux.org/)
- [Linux Mint](https://www.linuxmint.com/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Neovim](https://neovim.io/)
- [Biome](https://biomejs.dev/)
- [Phpmyadmin](https://www.phpmyadmin.net/)
- [Mariadb](https://mariadb.org/)
- [Cisco Packet Tracer](https://www.netacad.com/cisco-packet-tracer)
- [DBeaver](https://dbeaver.io/)
- [Omnisharp](https://www.omnisharp.net/)
- [Roslyn](https://en.wikipedia.org/wiki/Roslyn_(compiler))
- [Roslyn.nvim](https://github.com/seblyng/roslyn.nvim)
- [Jdtls](https://github.com/eclipse-jdtls/eclipse.jdt.ls)
- [Jdtls.nvim](https://codeberg.org/mfussenegger/nvim-jdtls)
- [LOVE2D](https://love2d.org/)
- [Dotnet](https://dotnet.microsoft.com/en-us/download)
- [Apache Netbeans](https://netbeans.apache.org/front/main/index.html)
- [Laravel Pint](https://laravel.com/docs/12.x/pint)
- [Conform.nvim](https://github.com/stevearc/conform.nvim)
- [Distrobox](https://distrobox.it/)
- [Flatpak](https://flatpak.org/)
- [XAMPP](https://www.apachefriends.org/)
- [Phpactor](https://github.com/phpactor/phpactor)
- [Phptools](https://www.devsense.com/en)
- [Java](https://www.java.com/en/)
- [C#](https://en.wikipedia.org/wiki/C_Sharp_(programming_language))
- [MySQL](https://www.mysql.com/)
- [Unity AUR package](https://aur.archlinux.org/packages/unityhub) Thank you so much!
- [Unity](https://unity.com/)
