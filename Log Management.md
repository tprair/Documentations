Log management monitoring in cloud management system, the Standard operating procedure (SOP) as the follows:
## Goal and scope of log management
-   Choose a log management tool that can collect, monitor, analyze, retain, index, search, and report on log data from various sources and formats

Some examples of web server and application server log locations are:

-   Apache web server: `/var/log/apache2` or `/var/log/httpd`
-   Nginx web server: `/var/log/nginx`
-   Tomcat application server: `/var/log/tomcat` or `/usr/share/tomcat/logs`
-   WebLogic application server: `<domain home>/servers/<server name>/logs` or `Admin Console->Servers->Server Name->Logging tab->Log file name`
-   WebSphere application server: `Admin Console->Servers->Server Name->Logging tab->JVM Logs link->Runtime tab->System out or System error logs`

### use commands like `cat`, `tail`, `less`, or `grep` to view or search the log files.