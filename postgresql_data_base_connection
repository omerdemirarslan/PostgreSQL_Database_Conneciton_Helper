""" Database Connection """
import os
import psycopg2
import psycopg2.extras
import psycopg2.extensions as psy_ext
import logging
import sys

from configs import *
from typing import Callable, Any, Iterable

logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)

formatter = logging.Formatter(log_format)

log_stream = logging.StreamHandler(sys.stdout)
log_stream.setLevel(logging.DEBUG)
log_stream.setFormatter(formatter)

logger.addHandler(log_stream)


class DataBaseHelper:
    """
    This Class Provides Connection Management From The User To The Database.
    """

    def __init__(self, **kwargs: dict):
        """
        Initialize Method
        :param user: required
        :param password: required
        :param host: required
        :param database: required
        :param port: Optional Default 5432
        """
        try:
            self._connection = psycopg2.connect(
                user=kwargs.get("user"),
                password=kwargs.get("password"),
                host=kwargs.get("host"),
                database=kwargs.get("database"),
                port=kwargs.get("port", 5432)
            )
            self._cursor = self._connection.cursor()
            self._connection.autocommit = kwargs.pop("auto_commit", True)
            self.last_execute = False

        except psycopg2.OperationalError as e:
            raise Exception("*Operational* Error: {}".format(e))

        except Exception as e:
            raise Exception("*Exception* Error: {}".format(e))

    def return_as_dict(self):
        """
        This Function Return Data Dictionary Format
        :return:
        """
        self._cursor.close()
        self._cursor = self._connection.cursor(cursor_factory=psycopg2.extras.RealDictCursor)

        return self

    def cursor(self) -> psy_ext.cursor:
        """
        This Function Return Connection
        :return: Postgre Connection
        """
        return self._cursor

    def connection(self) -> psy_ext.connection:
        """
        This Function Return Connection
        :return: Postgre Connection
        """
        return self._connection

    def __enter__(self):
        """
        This Function Return DBHelper
        :return: DBHelper
        """
        return self

    def __exit__(self, *args, **kwargs):
        """ This Function Will Close Connection and Cursor If Connection Is Open """
        if self._connection:
            self._cursor.close()
            self._connection.close()

    def execute(self, sql: str, params: tuple = ()) -> bool:
        """ This Function Execute SQL Query With Params """
        try:
            self._cursor.execute(sql, params)
            self.last_execute = True

            return True
        except Exception as e:
            logger.error(e)
            self.last_execute = False

        return False

    def fetchall(self) -> list:
        """ This Function Return SQL Queries Result List"""
        try:
            if self.last_execute:
                return self._cursor.fetchall()

            logger.info("Before calling the function, a successful SQL execution is required.")
        except Exception as e:
            logger.error("Error: {}".format(e))

        return []

    def fetchone(self) -> dict:
        """ This Function Return SQL Queries Result Dict """
        try:
            if self.last_execute:
                return self._cursor.fetchone()

            logger.info("Before calling the function, a successful SQL execution is required.")
        except Exception as e:
            logger.error("Error: {}".format(e))

        return {}
