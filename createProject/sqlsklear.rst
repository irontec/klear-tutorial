SQLs Klear
==========

Para crear las tablas necesarias para que funcione klear vamos a utilizar Phing.
Para ello crearemos un archivo que llamaremos 001-createKlearUsers.sql en el directorio /home/user/workspace/NuevoProyecto/phing/deltas/

El contenido del archivo variará en función de las necesidades de nuestro proyecto:

*  :ref:`Proyecto Simple <tablaklearUsersDefault>`
*  :ref:`Proyecto con Países y Timezones <tablaklearUsersPais>`


Si además se quieren configurar roles debemos añadir al archivo 001-createKlearUsers.sql las :ref:`tablas de Roles de KlearUsers <tablaRoles>`

.. attention::

   Una vez tengamos definido el fichero 001-createKlearUsers.sql, lo ejecutaremos mediante phing:

   .. code-block:: console

       $ phing migrate

.. _tablaklearUsers:

Tabla KlearUsers
----------------

.. _tablaklearUsersDefault:

Simple
******

Esta es la tabla sencilla para identificarse:

.. code-block:: mysql

   CREATE TABLE `KlearUsers` (
     `userId` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
     `login` varchar(40) NOT NULL,
     `email` varchar(255) NOT NULL,
     `pass` varchar(80) NOT NULL COMMENT '[password]',
     `active` tinyint(1) DEFAULT '1',
     `createdOn` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
     PRIMARY KEY (`userId`),
     UNIQUE KEY `login` (`login`),
     UNIQUE KEY `email` (`email`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='[entity]';
   insert into KlearUsers (login, pass, active) values ('admin','$2a$08$0hHHBX8So9JhU0a0SNnRCeAZcMEdAfn7T/pl/u/pESzBwztldhRnO', 1);

.. attention::
   | La última sentencia crea el siguiente usuario:
   | Usuario: admin
   | Password: 1234

.. _tablaklearUsersPais:

País y Timezone
***************


Agrega los campos countriId y timezoneId para determinar la procedencia de cada usuario a la hora de identificarse.

.. code-block:: mysql

   CREATE TABLE `Countries` (
      `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
      `name` varchar(255) DEFAULT NULL COMMENT '[ml]',
      `code` varchar(255) DEFAULT NULL,
      PRIMARY KEY (`id`),
      UNIQUE KEY `code` (`code`)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='[entity]';


   CREATE TABLE `Timezones` (
      `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
      `tz` varchar(255) DEFAULT NULL,
      `comment` varchar(150) DEFAULT '',
      `countryId` mediumint(8) unsigned DEFAULT NULL,
      PRIMARY KEY (`id`),
      UNIQUE KEY `tz` (`tz`),
      KEY `countryId` (`countryId`),
      CONSTRAINT `Timezones_ibfk_1` FOREIGN KEY (`countryId`) REFERENCES `Countries` (`id`) ON DELETE CASCADE ON UPDATE CASCADE
   ) ENGINE=InnoDB AUTO_INCREMENT=419 DEFAULT CHARSET=utf8 COMMENT='[entity]';

   CREATE TABLE `KlearUsers` (
     `userId` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
     `login` varchar(40) NOT NULL,
     `email` varchar(255) NOT NULL,
     `pass` varchar(80) NOT NULL COMMENT '[password]',
     `active` tinyint(1) DEFAULT '1',
     `createdOn` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
     `countryId` mediumint(8) unsigned DEFAULT NULL,
     `timezoneId` mediumint(8) unsigned DEFAULT NULL,
     PRIMARY KEY (`userId`),
     UNIQUE KEY `login` (`login`),
     UNIQUE KEY `email` (`email`),
     KEY `timezoneId` (`timezoneId`),
     KEY `countryId` (`countryId`),
     CONSTRAINT `KlearUsers_ibfk_2` FOREIGN KEY (`countryId`) REFERENCES `Countries` (`id`) ON DELETE SET NULL ON UPDATE CASCADE,
     CONSTRAINT `KlearUsers_ibfk_1` FOREIGN KEY (`timezoneId`) REFERENCES `Timezones` (`id`) ON DELETE SET NULL ON UPDATE CASCADE
   ) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8 COMMENT='[entity]';
   insert into KlearUsers (login, pass, active) values ('admin','$2a$08$0hHHBX8So9JhU0a0SNnRCeAZcMEdAfn7T/pl/u/pESzBwztldhRnO', 1);

.. attention::
   | La última sentencia crea el siguiente usuario:
   | Usuario: admin
   | Password: 1234


.. _tablaRoles:

Roles
-----

Para crear las tablas correspondientes para relacionar los usuarios a Roles, este es el código mysql.

.. code-block:: mysql


   -- Estructura de tabla para la tabla `KlearRoles`

   CREATE TABLE IF NOT EXISTS `KlearRoles` (
     `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
     `name` varchar(255) NOT NULL,
     `description` varchar(255) NOT NULL DEFAULT '',
     `identifier` varchar(255) NOT NULL,
     PRIMARY KEY (`id`),
     UNIQUE KEY `identifier` (`identifier`)
   ) ENGINE=InnoDB  DEFAULT CHARSET=utf8 COMMENT='[entity]';

   -- --------------------------------------------------------

   -- Estructura de tabla para la tabla `KlearRolesSections`

   CREATE TABLE IF NOT EXISTS `KlearRolesSections` (
     `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
     `klearRoleId` mediumint(8) unsigned NOT NULL,
     `klearSectionId` mediumint(8) unsigned NOT NULL,
     PRIMARY KEY (`id`),
     KEY `klearRoleId` (`klearRoleId`),
     KEY `klearSectionId` (`klearSectionId`)
   ) ENGINE=InnoDB  DEFAULT CHARSET=utf8 COMMENT='[entity]';

   -- --------------------------------------------------------

   -- Estructura de tabla para la tabla `KlearSections`

   CREATE TABLE IF NOT EXISTS `KlearSections` (
     `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
     `name` varchar(255) NOT NULL,
     `description` varchar(255) NOT NULL DEFAULT '',
     `identifier` varchar(255) NOT NULL,
     PRIMARY KEY (`id`),
     UNIQUE KEY `identifier` (`identifier`)
   ) ENGINE=InnoDB  DEFAULT CHARSET=utf8 COMMENT='[entity]';

   -- --------------------------------------------------------

   -- Estructura de tabla para la tabla `KlearUsersRoles`

   CREATE TABLE IF NOT EXISTS `KlearUsersRoles` (
     `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
     `klearUserId` mediumint(8) unsigned NOT NULL,
     `klearRoleId` mediumint(8) unsigned NOT NULL,
     PRIMARY KEY (`id`),
     KEY `klearUserId` (`klearUserId`),
     KEY `klearRoleId` (`klearRoleId`)
   ) ENGINE=InnoDB  DEFAULT CHARSET=utf8 COMMENT='[entity]';

   -- Restricciones para tablas volcadas

   -- Filtros para la tabla `KlearRolesSections`

   ALTER TABLE `KlearRolesSections`
     ADD CONSTRAINT `KlearRolesSections_ibfk_1` FOREIGN KEY (`klearRoleId`) REFERENCES `KlearRoles` (`id`) ON DELETE CASCADE,
     ADD CONSTRAINT `KlearRolesSections_ibfk_2` FOREIGN KEY (`klearSectionId`) REFERENCES `KlearSections` (`id`) ON DELETE CASCADE;

   -- Filtros para la tabla `KlearUsersRoles`

   ALTER TABLE `KlearUsersRoles`
     ADD CONSTRAINT `KlearUsersRoles_ibfk_1` FOREIGN KEY (`klearUserId`) REFERENCES `KlearUsers` (`userId`) ON DELETE CASCADE,
     ADD CONSTRAINT `KlearUsersRoles_ibfk_2` FOREIGN KEY (`klearRoleId`) REFERENCES `KlearRoles` (`id`) ON DELETE CASCADE;

   -- El siguiente código añadirá los permisos de administrador a tu usuario (con la ID = “1”) que previamente has creado.

   INSERT INTO `KlearRoles` (`id`, `name`, `description`, `identifier`) VALUES
   (1, 'Administrador', 'Usuario con permisos de administrador', 'admin'),
   (2, 'Usuario', 'Con roles para un usuario normal con restricciones.', 'user');


   INSERT INTO `KlearUsersRoles` (`id`, `klearUserId`, `klearRoleId`) VALUES
   (1, 1, 1);
