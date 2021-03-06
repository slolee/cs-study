## TCP 와 UDP 에 대해서 비교해서 설명해주세요.

TCP 와 UDP 는 OSI 7계층 중에서 4계층인 전송계층에 해당하는 프로토콜으로 통신을 하는 두 호스트가 서로 패킷을 어떻게 주고받을지에 대한 통신규약입니다.

TCP 는 송신자와 수신자가 커넥션을 맺어서 가상회선을 통해서 패킷을 주고받는 통신 방법으로, `통신 과정에서 오류제어, 흐름제어, 혼잡제어를 제공해 신뢰성을 제공`합니다. 하지만 커넥션을 맺기 위한 3-way 핸드쉐이크나 커넥션을 끊기 위한 4-way 핸드쉐이크 과정을 거치기 때문에 UDP 에 비해서 통신에 오버헤드가 붙게됩니다.

UDP 는 송신자와 수신자가 직접 연결하지 않고 데이터를 전달하는 통신 방법입니다. 연결을 맺지 않고 전송하기 대문에 중간에 데이터가 바뀌거나 유실되더라도 이를 해결하지 않습니다. 하지만 1:N, N:N 통신이 가능하고 전송속도가 TCP 에 비해서 빠르기 때문에 동영상 스트리밍과 같이 약간의 패킷 손실이 발생해도 괜찮은 곳에서 사용됩니다.

<br>

## 신뢰성이 보장되는 TCP 를 두고 UDP 를 사용하는 이유는 무엇일까요?

UDP 는 연결과정에서 사용되는 오버헤드를 피해 더 빠른 통신속도를 제공받고 싶거나 멀티캐스팅, 브로드캐스팅과 같이 1:N 의 통신을 구현하고 싶을 때 사용할 수 있습니다.

또 TCP 에서 제공하는 신뢰성이 별로 필요하지 않은 경우에도 UDP 를 사용할 수 있습니다.

<br>

## TCP 의 에러제어에 대해서 설명해주세요.

에러제어란 송신자가 수신자에게 패킷을 전송하는 과정에서 패킷이 유실되거나 에러가 발생한 경우 어떻게 제어할지에 대한 것입니다.

TCP 에서는 에러제어를 위해서 기본적으로 재전송을 기반으로 하는 여러가지 기능들을 제공합니다. 기본적으로 `송신자가 패킷을 전송하고 그에 대한 ACK 패킷이 일정시간 돌아오지 않거나 중복된 ACK 가 반복되어 돌아온다면 에러가 발생했음`을 알 수 있습니다.

이러한 에러제어를 구현하는 방법에는 `Stop and Wait 방식, Go Back N 방식, Selective-Reject 방식`이 있습니다.

`Stop and Wait 방식은 송신자가 하나의 패킷을 보내면 보낸 패킷에 대한 수신자의 응답을 받아야 다음 패킷을 전달하는 방식`입니다. 일정시간 응답이 오지 않거나 에러가 발생했다는 응답이 오게되면 해당 패킷을 재전송하게 됩니다.

`Go Back N 방식은 송신자가 패킷을 전송할 때 수신측이 데이터를 잘못 받거나 못받았을 경우 해당 패킷부터 다시 재전송하는 방법`입니다. 송신측의 슬라이딩 윈도우는 보냈지만 응답을 받지 못한 것을 기억하고 있고 일정시간동안 ACK 을 받지 못하거나 NAK 패킷을 받게된다면 해당 패킷부터 재전송하게 됩니다. NAK 패킷은 수신자가 패킷을 받았을때 에러가 있거나 특정 패킷이 분실되어서 받지못하고 다음 패킷을 받게된 경우에 송신자에게 보내게 됩니다.

`Selective-Reject 방식은 잘못받은 패킷 번호만 다시 재전송하는 방식`입니다. 이 방법이 Go Back N 방식보다 효율은 좋지만 수신측에서 패킷을 확인하기 위한 추가적인 정렬된 배열이 필요하고 구현방식이 복잡하다는 문제가 있습니다.

<br>

## TCP 의 흐름제어에 대해서 설명해주세요.

송신자와 수신자의 데이터 처리 속도를 제어하기 위한 기법으로 수신자가 감당할 수 없는 만큼의 패킷을 송신자가 전달하지 않도록 제어하는 기법입니다.

이러한 흐름제어를 구현하는 방법에는 `Stop and Wait 방식, Sliding Window 방식`이 있습니다.

Stop and Wait 는 송신자가 하나의 패킷을 보내면 수신자가 받았다는 응답을 받아야 그 다음 패킷을 전송하는 방법입니다. 

Sliding Window 는 수신자가 지정한 Window 크기만큼 송신자와 수신자가 버퍼를 가지고 수신자의 응답을 하나씩 받지 않더라도 버퍼 크기만큼 패킷을 전달하는 방법입니다.

<br>

## Sliding Window 가 어떻게 구현되는지 간략하게 설명해주세요.

`송신자는 TCP Connection 과정에서 수신자가 한번에 처리할 수 있는 버퍼의 크기 즉, Window 크기를 전달받습니다.` 그리고 송신자는 수신자에게 `Window 크기 만큼의 패킷을 전송`합니다. 이후 `수신자가 패킷을 받아 처리하고 추가로 처리가능한 크기를 송신자에게 응답과 함께 전달`합니다.

송신자는 수신자가 처리가능하다고 한 크기만큼의 패킷을 추가로 전달합니다. 이때 송신자가 전달받은 크기만큼 버퍼를 슬라이딩하게 되는데 이러한 이유에서 슬라이딩 윈도우라고 합니다.

<br>

## TCP 의 혼잡제어에 대해서 설명해주세요.

TCP 의 혼잡제어란 네트워크망이 혼잡해져서 충돌이 발생하는 것을 줄이고 한정된 자원을 잘 분배해 통신이 원활하게 이루어질 수 있도록 하는 것입니다.

이러한 혼잡제어는 `합 증가 곱 감소방식과 Slow Start 방식`으로 구현됩니다.

`합 증가 및 곱 감소 방식은 Window 사이즈를 전송시마다 1씩 증가시키며 보내고, 만약 패킷 전송에 실패하거나 타임아웃이 발생하게되면 Window 사이즈를 절반으로 줄이는 방식`을 말합니다. 이 방법은 `전송 속도를 올리는데 걸리는 시간이 너무 길다는 단점`이 있습니다.

`Slow Start 방식은 패킷이 문제없이 도착했다는 ACK 패킷을 받게되면 Window 사이즈를 1 늘리는 방식`입니다. 즉 하나의 슬라이딩 Window 주기가 끝나면 Window 사이즈는 2배가 됩니다. 그리고 `패킷 전송에 실패하거나 타임아웃이 발생하게되면 Window 크기를 1로 초기화`합니다. 

하지만 계속해서 이렇게 Window 크기를 늘리게되면 기하급수적으로 늘어나 제어가 힘들어지기 때문에 특정 지점을 임계점으로 지정해 해당 지점까지만 Slow Start 방식을 사용하고 이후에는 Window 크기를 1 씩 증가시키는 방식을 사용해야 합니다.

<br>

## 3-way 핸드쉐이크와 4-way 핸드쉐이크에 대해서 설명해주세요.

3-way Handshake 는 TCP 에서 두 호스트가 통신을 위해서 TCP Connection 을 맺는 과정을 의미합니다. 

송신 호스트가 난수로 이루어진 SYN 패킷을 통해 수신 호스트에게 통신 요청을 보내게 되고, 이를 받은 수신 호스트는 ACK 와 SYN 패킷을 통해 요청에 대한 응답을 함과 동시에 반대로 통신에 대한 요청을 보내게 됩니다. 이를 받은 송신 호스트는 ACK 패킷을 통해 요청에 대한 응답을 함으로써 양 호스트가 통신이 가능하다는 것이 확인되고 이후부터 맺어진 TCP Connection 을 통해 데이터를 주고받습니다.

4-way Handshake 는 TCP 에서 두 호스트가 통신을 마치고 TCP Connection 을 끊는 과정을 의미합니다.

연결을 종료하고자하는 송신 호스트가 수신 호스트에게 FIN 패킷을 보내고 FIN 패킷을 받은 호스트는 ACK 패킷으로 응답합니다. 그리고 보내야할 남은 데이터가 있다면 전부 보낸 뒤에 반대로 FIN 패킷을 보내 연결을 종료하자고 요청합니다. FIN 패킷을 송신 호스트는 ACK 패킷을 보낸 후 Wait 상태에 돌입해 아직 전달받지 못한 데이터를 전달받기 위한 상태로 들어가고, 일정시간 후에 TCP Connetion 을 닫습니다. 반대 수신 호스트는 송신 호스트에게 ACK 패킷을 받는 순간 TCP Connection 을 닫습니다.

<br>

## TCP Connection 을 끊는 과정을 3번으로 할 수 없는 이유가 있을까요?

연결을 맺는 과정에서는 연결 요청에 대한 응답인 ACK 와 반대로의 연결 요청인 SYN 를 하나의 패킷으로 전송할 수 있어 3번의 과정으로 연결이 가능합니다.

하지만, 연결을 끊자는 FIN 요청이 왔을 때 아직 더 송신해야 할 데이터가 남아있을수 있기 때문에 우선 FIN 에 대한 응답인 ACK 패킷을 먼저 전송하고, 데이터를 다 보낸후에 FIN 패킷을 보내야 합니다.

따라서 3번의 과정으로 할 수 없고 총 4번의 과정이 필요하게 됩니다.

<br>

## TCP 가 데이터를 전송할 때 데이터를 패킷으로 분리하게 되는데 이 과정에 대해서 설명해주실수 있나요?

TCP 는 데이터를 분할하는 단위인 MSS 로 나눠서 세그먼트라는 단위로 나눕니다. 세그먼트는 TCP 에서 분할된 데이터ㄱ에 헤더가 붙어 캡슐화된 전송계층의 패킷을 의미합니다.

이렇게 분할된 데이터에 번호를 부여하여 전송과정에서 순서가 바뀌더라도 수신지에서 원래의 데이터 형태로 재조립할 수 있도록 합니다. `정리하자면 분할된 데이터에 순서 번호, 송신지 IP, Port 와 같은 정보를 추가해 세그먼트로 만들고 이를 인터넷 계층으로 보내 목적지에 전달합니다.`

순서 번호는 패킷의 SYN 이라는 플래그를 의미하는 것으로 최초의 3-way Handshake 과정에서는 임의의 난수를 사용하고 이후에는 패킷에 포함된 데이터의 크기를 더해 SYN 을 증가시켜 패킷을 전송합니다.

<br>

## IP 단편화에 대해서 설명해주세요.

단편화는 IP 패킷의 크기를 MTU 의 크기와 비교해 MTU 크기 이하로 만드는 것을 의미합니다. 단편화는 IP 패킷의 크기가 MTU 보다 클 경우 발생합니다. 단편화는 전송지에서 뿐만아니라 경유하는 동안에도 발생할 수 있으며, UDP 는 데이터의 순서를 보장하지 않기 때문에 목적지에 도착한 후 재조립(리어셈블링)됩니다.

경유지에서 단편화가 발생하는 이유는 경유지마다 MTU 가 다를 수 있기 때문입니다.

<br>

## 