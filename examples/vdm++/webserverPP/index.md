---
layout: default
title: webserver
---

~~~

#******************************************************
~~~
###file.vdmpp

{% raw %}
~~~
class File
  instance variables
    data: seq of char := ""
  operations 
    public
end File
~~~{% endraw %}

###filename.vdmpp

{% raw %}
~~~

class Filename
  operations
   -- get_file models OS operation of taking a location and returning
   -- put_file models OS operation of storing given file at a given
end Filename
~~~{% endraw %}

###url.vdmpp

{% raw %}
~~~

class URL
  operations
    -- for a given configuration, translate maps this URL to the 

end URL
~~~{% endraw %}

###webserver.vdmpp

{% raw %}
~~~

class Webserver
  functions

    -- execute_script models OS execution of a CGI script generating
  instance variables
  operations
    ExecuteCGI : URL ==> File
    RetrieveURL : URL ==> File
    UploadFile : File * URL ==> ()
    ServerBusy : () ==> File
    DeleteURL : URL ==> ()
  sync
    per RetrieveURL => #active(RetrieveURL) + #active(ExecuteCGI) < 10;
end Webserver


~~~{% endraw %}
