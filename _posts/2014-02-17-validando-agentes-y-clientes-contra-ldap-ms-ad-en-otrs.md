---
title: "Validando Agentes y Clientes contra LDAP (Ms AD) en OTRS"
date: 2014-02-17 18:27:00.002
tags: ["cliente", "agente", "otrs", "agent", "active directory", "ldap", "directorio activo", "microsoft", "customer"]
category: professional
---

Hace unos pocos días me toco configurar OTRS. En la fabrica utilizan OTRS para gestionar las incidencias informáticas y tienen una versión obsoleta. En la versión que he configurado he adaptado nuevas funcionalidades como que, aparte de que los Clientes se validen contra el Directorio Activo, ahora tambien los agentes puedan hacerlo.  
  
Os adjunto mi fichero de configuracion (Config.pm)  
  

```
   # ---------------------------------------------------- #  
   # insert your own config settings "here"        #  
   # config settings taken from Kernel/Config/Defaults.pm #  
   # ---------------------------------------------------- #  
   # $Self->{SessionUseCookie} = 0;  
   # $Self->{CheckMXRecord} = 0;  
 # FIRST the authentication.  
   # Customer LDAP authentication backend.  
   $Self->{'Customer::AuthModule2'} = 'Kernel::System::CustomerAuth::LDAP';  
   $Self->{'Customer::AuthModule::LDAP::Host2'} = 'LDAPmachinename';  
   $Self->{'Customer::AuthModule::LDAP::BaseDN2'} = 'DC=company,DC=local';  
   $Self->{'Customer::AuthModule::LDAP::UID2'} = 'sAMAccountName';  
   # Check if member of AD group before customer login.  
   # Nested groups doesn't seem to work. Have to use a global one that most people are member of, "Domain Users" won't work, as it's a special group.  
   # Disable until further notice. Perhaps nested groups will work someday.  
   # Note to self: Try LDAP query for nested members (memberof:1.2.840.113556.1.4.1941:=<Full DN>).  
   $Self->{'Customer::AuthModule::LDAP::GroupDN2'} = 'CN=otrsgroup,OU=OTRS,OU=customers,DC=company,DC=local';  
   $Self->{'Customer::AuthModule::LDAP::AccessAttr2'} = 'member';  
   $Self->{'Customer::AuthModule::LDAP::UserAttr2'} = 'DN';  
   # LDAP Bind credentials.  
   $Self->{'Customer::AuthModule::LDAP::SearchUserDN2'} = 'CN=myuser,OU=SSII,OU=IT,DC=company,DC=local';  
   $Self->{'Customer::AuthModule::LDAP::SearchUserPw2'} = 'mypass';  
   # We only need to see users, not computers, and no disabled users either.  
   $Self->{'Customer::AuthModule::LDAP::AlwaysFilter2'} = '(&(samAccountType=805306368)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))';  
 # THEN the data backend.  
   # Customer LDAP data backend.  
 $Self->{CustomerUser2} = {  
   # Just the display name in OTRS.  
   Name => 'LDAP - Backend',  
   Module => 'Kernel::System::CustomerUser::LDAP',  
   Params => {  
     Host => 'LDAPmachinename',  
     BaseDN => 'DC=company,DC=local',  
     # Search Scope.  
     SSCOPE => 'sub',  
     # LDAP Bind credentials.  
     UserDN => 'CN=myuser,OU=SSII,OU=IT,DC=company,DC=local',  
     UserPw => 'mypass',  
     # This is needed to avoid issues with ae, oe, aa. Dunno what happens or if it's entirely correct.  
     SourceCharset => 'utf-8',  
     DestCharset => 'utf-8',  
     # We only need to see users, not computers, and no disabled users either.  
     AlwaysFilter => '(&(samAccountType=805306368)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))',  
   },  
   CustomerKey => 'sAMAccountName',  
 #  CustomerID => 'mail',  
   CustomerID => 'sAMAccountName',  
   # Show fields when searching customers in the admin interface.  
   CustomerUserListFields => ['sAMAccountName', 'cn', 'mail'],  
 #  CustomerUserListFields => ['cn', 'mail'],  
   # Search in these fields in LDAP.  
   CustomerUserSearchFields => ['sAMAccountName', 'cn', 'mail'],  
   CustomerUserPostMasterSearchFields => ['mail'],  
   CustomerUserNameFields => ['givenname', 'sn'],  
   CustomerUserSearchListLimit => '1000',  
   Map => [  
     # Note: Login, Email and CustomerID are required!  
     # 'var', 'frontend', 'storage', shown, required, 'storage-type'  
 #    [ 'UserTitle', 'Title', 'title', 1, 0, 'var' ],  
     [ 'UserLogin', 'Login', 'sAMAccountName', 1, 1, 'var' ],  
     [ 'UserFirstname', 'Firstname', 'givenname', 1, 1, 'var' ],  
     [ 'UserLastname', 'Lastname', 'sn', 1, 1, 'var' ],  
     [ 'UserEmail', 'Email', 'mail', 1, 1, 'var' ],  
     [ 'UserCustomerID', 'CustomerID', 'mail', 0, 1, 'var' ],  
     [ 'UserCustomerIDs', 'CustomerIDs', 'customer_ids', 1, 0, 'var'],  
     [ 'UserPhone', 'Phone', 'telephonenumber', 1, 0, 'var' ],  
     [ 'UserDepartment', 'Departamento', 'department', 1, 0, 'var' ],  
 #    [ 'UserAddress', 'Address', 'postaladdress', 1, 0, 'var' ],  
     [ 'UserComment', 'Comment', 'description', 1, 0, 'var' ],  
   ],  
 };  
 #Authenticate agents against a DB backend  
   $Self->{'AuthModule3'} = 'Kernel::System::Auth::DB';  
 # Authenticate agents against MS AD backend  
   $Self->{'AuthModule4'} = 'Kernel::System::Auth::LDAP';  
   $Self->{'AuthModule::LDAP::Host4'} = 'LDAPmachinename';  
   $Self->{'AuthModule::LDAP::BaseDN4'} = 'dc=company,dc=local';  
   $Self->{'AuthModule::LDAP::UID4'} = 'sAMAccountName';  
   # Check if the user is allowed to auth in a posixGroup  
   # (e. g. user needs to be in a group OTRS_Agents to use otrs)  
   $Self->{'AuthModule::LDAP::GroupDN4'} = 'CN=otrsAgent,OU=OTRS,OU=Employees,DC=company,DC=local';  
   $Self->{'AuthModule::LDAP::AccessAttr4'} = 'member';  
   $Self->{'AuthModule::LDAP::UserAttr4'} = 'DN';  
   # Bind credentials to log into AD  
   $Self->{'AuthModule::LDAP::SearchUserDN4'} = 'CN=myuser,OU=SSII,OU=IT,DC=company,DC=local';  
   $Self->{'AuthModule::LDAP::SearchUserPw4'} = 'mypass';  
   # in case you want to add always one filter to each ldap query, use  
   # this option. e. g. AlwaysFilter => '(mail=*)' or AlwaysFilter => '(objectclass=user)'  
   $Self->{'AuthModule::LDAP::AlwaysFilter4'} = '(&(samAccountType=805306368)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))';  
   # in case you want to add a suffix to each login name, then  
   # you can use this option. e. g. user just want to use user but  
   # in your ldap directory exists user@domain.  
   #$Self->{'AuthModule::LDAP::UserSuffix'} = '';  
   # Net::LDAP new params (if needed - for more info see perldoc Net::LDAP)  
 #  $Self->{'AuthModule::LDAP::Params'} = {  
 #    port => 389,  
 #    timeout => 120,  
 #    async => 0,  
 #    version => 3,  
 #  };  
   # Now sync data with OTRS DB  
   $Self->{'AuthSyncModule5'} = 'Kernel::System::Auth::Sync::LDAP';  
   $Self->{'AuthSyncModule::LDAP::Host5'} = 'LDAPmachinename';  
   $Self->{'AuthSyncModule::LDAP::BaseDN5'} = 'dc=company, dc=local';  
   $Self->{'AuthSyncModule::LDAP::UID5'} = 'sAMAccountName';  
   $Self->{'AuthSyncModule::LDAP::SearchUserDN5'} = 'CN=myuser,OU=SSII,OU=IT,DC=company,DC=local';  
   $Self->{'AuthSyncModule::LDAP::SearchUserPw5'} = 'mypass';  
   $Self->{'AuthSyncModule::LDAP::AccessAttr5'} = 'member';  
      $Self->{'AuthSyncModule::LDAP::UserAttr5'} = 'DN';  
   $Self->{'AuthSyncModule::LDAP::AlwaysFilter5'} = '(&(samAccountType=805306368)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))';  
   $Self->{'AuthSyncModule::LDAP::UserSyncMap5'} = {  
     # DB -> LDAP  
     UserFirstname => 'givenName',  
     UserLastname => 'sn',  
     UserEmail   => 'mail',  
     UserLogin => 'sAMAccountName',  
     UserPhone   => 'telephonenumber',  
   };  
  # This section tries to sync groups to AD  
   # Attributes needed for group syncs  
   # (attribute name for group value key)  
 #  $Self->{'AuthSyncModule::LDAP::AccessAttr'} = 'member';  
   # (attribute for type of group content UID/DN for full ldap name)  
 #  $Self->{'AuthSyncModule::LDAP::UserAttr'} = 'DN';  
   # AuthSyncModule::LDAP::UserSyncInitialGroups  
   # (sync following group with rw permission after initial create of first agent  
   # login)  
 #  $Self->{'AuthSyncModule::LDAP::UserSyncInitialGroups'} = [  
 #    'users',  
 #  ];       
   # AuthSyncModule::LDAP::UserSyncRolesDefinition  
   # (If "LDAP" was selected for AuthModule and you want to sync LDAP  
   # groups to otrs roles, define the following.)  
   $Self->{'AuthSyncModule::LDAP::UserSyncRolesDefinition5'} = {  
     # ldap group  
 #    'cn=agent,o=otrs' => {  
 #      # otrs role  
 #      'role1' => 1,  
 #      'role2' => 0,  
 #    },  
     'CN=otrsAdmin,OU=OTRS,OU=Employees,DC=company,DC=local' => {  
       'Admin' => 1,  
     },  
     'CN=otrsrol1,OU=OTRS,OU=Employees,DC=company,DC=local' => {  
       'rol1' => 1,  
     },  
     'CN=otrsrol2,OU=OTRS,OU=Employees,DC=company,DC=local' => {  
       'rol2' => 1,  
     },  
     'CN=otrsrol3,OU=OTRS,OU=Employees,DC=company,DC=local' => {  
       'orl3' => 1,  
     },  
     'CN=otrsITSupervisor,OU=OTRS,OU=Employees,DC=company,DC=local' => {  
       'IT Supervisor' => 1,  
     }  
   };
```
