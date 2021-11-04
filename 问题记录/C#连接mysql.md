4、[C#连接数据库MySql命令](http://www.cnblogs.com/myyan/p/3746410.html)

using System;
using MySql.Data.MySqlClient;

namespace WindowsFormsApp1
{
    class MysqlUtils
    {
        #region Mysql连接字符串
        private static string connetStr = "server=127.0.0.1;port=3306;user=root;password=toor; database=company;";
        // server=127.0.0.1/localhost 代表本机，端口号port默认是3306可以不写
        private static MySqlConnection conn = new MySqlConnection(connetStr);
        #endregion
        public static MySqlConnection getConn()
        {
            return conn;
        }
    }
}