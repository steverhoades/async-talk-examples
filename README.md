Overview
===============
The following examples are used in my Asynchronous PHP talks.  Mileage may vary.  The following are the dependencies.

+ PHP 5.6
+ ReactPHP
+ Ratchet
+ PECL Event


Server Sent Events Demo
---------------

To start the demo
    
    cd reactphp/ratchet-examples/server-sent-events
    php server.php > /dev/null 2>&1 &
    

Detect Filesystem Changes and Auth-refresh
---------------
Dependencies:
+ inotify PHP extension 

To start the demo
    
    cd reactphp/ratchet-examples/revealjs/
    php <path_to>/reactphp/ratchet-examples/websockets/client_refresh_server.php > /dev/null 2>&1 &
    

Multi-user Sketch Pad and Check via WebSockets
----------------
To start the demo

    php reactphp/ratchet-examples/websockets/drawing/draw_server.php > /dev/null 2>&1 &

Mini "Game" Board with Chat via WebSockets
-----------------
To start the demo

    cd reactphp/ratchet-examples/wamp/simple_canvas_game/
    php <path_to>/reactphp/ratchet-examples/wamp/simple-canvas-game/server.php > /dev/null 2>&1 &


Proxy Server
-----------------
These demos were run using HAProxy.  

    global
        log 127.0.0.1   local0
        maxconn 10000
        user    haproxy
        group   haproxy
        daemon
    
    defaults
        mode            http
        log         global
        option          httplog
        retries         3
        backlog         10000
        timeout client      30s
        timeout connect     30s
        timeout server      30s
        timeout tunnel      3600s
        timeout http-keep-alive 1s
        timeout http-request    15s
    
    frontend public
        bind        *:80
        acl     is_websocket hdr(Upgrade) -i WebSocket
        use_backend ws if is_websocket #is_websocket_server
        default_backend www
    
    backend ws
        server  ws1 127.0.0.1:8080
    
    backend www
        timeout server  30s
        server  www1    127.0.0.1:81

