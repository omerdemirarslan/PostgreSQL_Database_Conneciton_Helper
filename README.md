# PostgreSQL_Database_Conneciton_Helper
The Script In This Repo Is  Helpful PostgreSQL Database Connection.

# How To Use PostgreSQL Database Connection

## Step-1: Import Classfrom 
cameralyze.helpers.database.postgre import DBHelper

## Step-2: Create New Connection Your Database
Like This -->
    self.client_connection = DBHelper(**self.__your_client_database_connection())
    
    def __get_your_database_connection(self) -> dict:
        """
        This Function Connects To Client Database
        :return:
        """
        return dict(
            user="your database user name",
            host="your database host name",
            password="your database password",
            database="your database name",
        )
        
Or Like This -->

     with DBHelper(**self.__your_client_database_connection()) as db:
         db.run(sql="YOUR_SQL_QUERY", params="YOUR_PARAMS")
         result = db.fetchone()/fetchall()
         
