[[Маска подсети (subnet mask)]] - инструкция как читать IP adress. Допустим есть сеть,
<mark style="background: #FFB8EBA6;">192.168.5</mark><mark style="background: #ADCCFFA6;">.33</mark> розовый цвет относится к сети, а синий к номеру хоста в сети. Но также этот же IP адрес может принадлежать другому устройству в другой сети <mark style="background: #FFB8EBA6;">192.168</mark><mark style="background: #ADCCFFA6;">.5.33</mark>. Для того чтобы не было такой путаницы есть **маска подсети** чтобы точно понимать где заканчивается <mark style="background: #FFB8EBA6;">адрес сети</mark> а где начинается <mark style="background: #ADCCFFA6;">номер хоста </mark>. 
**Маска подсети** может записывать в десятичном виде <mark style="background: #BBFABBA6;">255.255.255.</mark><mark style="background: #FFB8EBA6;">0</mark> и через черту <mark style="background: #BBFABBA6;">192.168.5.33</mark>/24 оба варианта означают что в адресе 192.168.5.33 первые 3 октета относятся к сети а оставшийся относится к номеру хоста.


к примеру у IP адреса 192.168.5.33 маска подсети будет <mark style="background: #BBFABBA6;">255</mark>.<mark style="background: #BBFABBA6;">255</mark>.<mark style="background: #BBFABBA6;">255</mark>.<mark style="background: #FFB8EBA6;">0</mark> таким образом где <mark style="background: #BBFABBA6;">255</mark> остается то же значение из IP адреса а <mark style="background: #FFB8EBA6;">0</mark> относится к адресу подсети так же маска подсети может иметь такой вид - 192.168.5.33/<mark style="background: #ABF7F7A6;">24</mark> это значит что <mark style="background: #ABF7F7A6;">24</mark> бита относятся к сети, а оставшиеся 12 бит относятся к узлу (как мы помним в[[IP adress| IP adress]] всего 32 bit ) 
Классы  
- А- значит что первые 8 битов зарезервированы под сеть а остальные под узел 255.0.0.0
- В- значит что первые 16 битов зарезервированы под сеть а остальные под узел 255.255.0.0
- С- значит что первые 24 битов зарезервированы под сеть а остальные под узел 255.255.255.0
- D- для многоадресной рассылки
- E- зарезервировано