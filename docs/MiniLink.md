# MiniLink

## Variables

### ser : `instance`

```python
ser:serial = None
```

`Serial()` 인스턴스.<br>
USB 장치와 시리얼 통신을 관리함.


### packet : `dict`

```python
packet = {
    'Header' : 0, 'Length' : 0, 'SEQ' : 0, 'MSG ID' : 0, 
    'data' : numpy.zeros(1024, int), 
    'CRC' : 0
}
```

수신 받은 데이터의 패킷이 저장됨


### __cnt : `int` & __MSG_ID : `int`

```python
__cnt:int = 0
__MSG_ID:int = None
```

- `__cnt` : 수신 받은 바이트의 수를 저장함.
- `__MSG_ID` : 화면에 출력하기 위해 선택한 MSG ID를 저장함.


### xmlHandler : `instance`

```python
xmlHandler = XmlHandler()
```

`XmlHander()` 인스턴스

### msgs_dict : `dict` & msgs_dict_ori : `dict`

```python
msgs_dict : dict= {}
msgs_dict_ori : dict= {}
# { id : [name, instance, count] }
```

message 목록이 저장되는 딕셔너리<br>
`msgs_dict`는 현재 수신되는 msg만 저장하고, `msgs_dict_ori`는 모든 msg를 저장한다.





## Fuctions

### List

```python
def __init__(self)
def connect(self, port, baudrate:int=115200, MSG_ID:int=None)
def chooseMessage(self, id:int)
def getIDfromName(self, name:str)
def getMessageFrequency(self, id:int)
def getMessageList(self, selectId:bool=False, selectNames:bool=False, selectCounts:bool=False)
def getMessageName(self)
def getMessageColumnNames(self)
def getMessageColumnTypes(self)
def getPayload(self)
def read(self, enPrint:bool=False, enLog:bool=False)
def send(self, data : list)
def updateMessageList(self)
def __calculate_crc(self, data, length)
def __checkCRC(self)
def __byte2int(self, x)
def __readByte(self, packet:dict)
```

### __init__()

```python
def __init__(self)
```

클래스 생성자.<br>
`xmlHandler.loadMessageListFromXML()` 함수를 호출함

### connect()

```python
def connect(self, port, baudrate:int=115200, MSG_ID:int=None)
    '''
    # connect()
    장치에 연결한다.

    Params :
        port - 연결할 포트
        baudrate `int` - 보드레이트
        MSG_ID `int` - Message ID

    Returns :
        0 - 선택됨
        -1 - 유효하지 않은 메세지 번호
    '''
```

### chooseMessage()

```python
def chooseMessage(self, id:int)
    '''
    # chooseMessage()
    화면에 표시할 메세지를 선택한다.
    갱신하는 함수는 `updateMessageList()`

    Params :
        id `int` - Message ID

    Returns :
        0 - 선택됨
        1 - 유효하지 않은 메세지 번호
    '''
```

### getIDfromName()

```python
def getIDfromName(self, name:str):
    '''
    # getIDfromName()
    name으로부터 message의 ID를 구한다.

    Params :
        name `str` - message의 이름
    Returns :
        id `int` - message의 id
        None - 값이 없을 때
    '''
```

### getMessageFrequency()

```python
def getMessageFrequency(self, id:int=None):
    '''
    # getMessageFrequency()
    수신 받은 message의 빈도를 출력한다.

    Params :
        (optical) id `int` - Message ID
    Returns :
        (default) `[names, counts]`
        (others) `counts`
        names `list` - message의 이름
        counts `list` - message의 빈도 값
    '''
```

### getMessageList()

```python
def getMessageList(self, selectId:bool=False, selectNames:bool=False, selectCounts:bool=False):
    '''
    # getMessageList()
    수신 받은 Message의 목록을 반환한다.
    갱신하는 함수는 `updateMessageList()`

    Params :
        (optical) selectId `bool` - MSG ID 값을 리스트로 반환
        (optical) selectNames `bool` -  Message의 이름 값을 리스트로 반환
        (optical) selectCounts `bool` -  Message의 총 빈도 값을 리스트로 반환
    Returns :
        (default) self.msgs_dict `dict` `{ id : [ name, instance, count ] }`
        (selectId) data `list` `[ids]`
        (selectName) data `list` `[names]`
        (selectCounts) data `list` `[couts]`
        (selectId|selectName) data `list` `[id, names]`
    '''
```

### getMessageName()

```python
def getMessageName(self, id:int=None):
    '''
    # getMessageName()
    Message의 이름을 반환한다.

    Params :
        id `int` - 원하는 MSG ID. 기본값은 None(현재 반환하는 값)
    Returns :
        name `str`
    '''
```

### getMessageColumnNames()

```python
def getMessageColumnNames(self, id:int=None):
    '''
    # getMessageColumnNames()
    Message의 속성들의 이름 출력한다.

    Params :
        id `int` - 원하는 MSG ID. 기본값은 None(현재 반환하는 값)
    Returns :
        name `list(str)`
    '''
```

### getMessageColumnTypes()

```python
def getMessageColumnTypes(self, id:int=None):
    '''
    # getMessageColumnTypes()
    Message의 속성들의 타입 출력한다.

    Params :
        id `int` - 원하는 MSG ID. 기본값은 None(현재 반환하는 값)
    Returns :
        name `list(str)`
    '''
```

### getPayload()

```python
def getPayload(self):
    '''
    ## getPayload()
    수신 받은 패킷 중 Payload 부분만 출력한다.
    
    Returns :
        Payload `list`
    '''
```

### read()

```python
def read(self, enPrint:bool=False, enLog:bool=False):
    '''
    # read()
    한 패킷을 수신한다.

    Params :
        enPrint `bool` - print 유무
        enLog `bool` - Log 저장 유무

    Returns :
        unpacked_data `list` - 자료형에 맞게 가공된 payload
        None - 원하는 메세지가 아닐 때
    '''
```

### send()

```python
def send(self, msg_id:int, payload:list):
    '''
    # send()
    FC에 message 전송한다.

    Params :
        msg_id `int`
        payload `list` 
    
    Returns :
        0 : 정상 송신
        1 : 송신 에러
    '''
```

### updateMessageList()

```python
def updateMessageList(self):
    '''
    # updateMessageList()
    수신 받은 Message의 목록을 갱신하고 반환 한다.

    Returns :
        self.msgs_dict `dict`
        {id:[name, instance, count]}
    '''
```

### __calculate_crc()

```python
def __calculate_crc(self, data, length):
    length : int= self.packet['Length']
    data : numpy.array = self.packet['data']

    rxCRC = data[length-1]<<8 | data[length-2] 
    retval = self.__calculate_crc(data, length)

    return retval == rxCRC
```

### __byte2int

```python
def __byte2int(self, x):
    return int("0x"+str(x),16)
```

### __readByte()

```python
def __readByte(self, packet:dict):
    '''
    # __readByte()
    byte 단위로 데이터 받아옴

    Returns :
        -1 - 한 바이트를 정상 수신함.
        0 - 한 패킷을 정상 수신함.
        1 - 한 패킷을 정상 수신하였으나 CRC 오류.
        2 - 패킷의 길이 오류
        3 - 패킷이 MiniLink가 아님
        4 - 입력 값이 바이트가 아님
    '''
```

###

```python

```