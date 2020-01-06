[Folly](https://github.com/facebook/folly)

ip + port = SocketAddr

```C++
folly::SocketAddress localHttp("127.0.0.1", 8080);
localHttp.setPort(8888);
```

### UDP
- пакаты до 1000байт
- `SendTo(data, (ip, port))`, отправляет пакет в сеть
- `Listen((ip, port)) + Recv() -> (data, (ip, port))`, принимает
- гарантий доставки нет
- DNS работает поверх UDP

### TPC
- передает поток байт скрывая потери в сети
- требует установки соединения
- TCP соединение - `два` потока байт
- Идентификатор соединения `(src_ip, src_port, dst_ip, dst_port)`

Установка соединения:
1. Сервер `Listen((ip, port))`
2. Сервер `Accept`
3. Клиент `Dial((ip, port))`
4. `Accept` и `Dial` завершаются, возвращая объект соединения

### Debugging TCP
```
nc $IP $PORT     # устанавливае TCP соединение
nc -l $PORT      # слушает входящие соединения
nc -v $IP $PORT  # дополнительные логи
```

```
# Более новый аналог netstat
ss -ltp  # показать слушающие (-l) tcp (-t) сокеты
ss -l    # список открытых tcp соединений
```
