# Eight Design Patterns

## Creational Patterns
### Factory
```py
    '''
    If it takes a list of ingredients to create a burger, use a factory to instantiate a burger
    Return what type of burger is desired as they are all produced from a foundational structure
    '''
    #General Burger class
    class Burger:
        def __init__(self, ingredients):
            self.ingredients=ingredients
        def __str__(self):
            return f'{self.ingredients}'
    #Creates different type of Burger depending on method invoked
    class BurgerFactory:
        def createCheeseBurger(self):
            ingredients=['bun','cheese','patty']
            return Burger(ingredients)
        def createDeluxeBurger(self):
            ingredients=['bun','tomato','lettuce','cheese','patty']
            return Burger(ingredients)

    CheeseBurger=createCheeseBurger()
    print(CheeseBurger.__str__())
```

### Builder
```py
    '''
    Create Builder with individual methods for adding elements to object with reference to builder

    Build method returns final product
    Then instantiate final method that builds with each method -> build

    ! Protocol Buffers !
    '''
    class Burger:
        def __init__(self):
            self.cheese=None
            self.buns=None
            self.patty=None
        def setBuns(self, bunStyle):
            self.buns=bunStyle
        def setPatty(self, pattyStyle):
            self.patty=pattyStyle
        def setCheese(self, cheeseStyle):
            self.cheese=cheeseStyle
    class BurgerBuilder:
        def __init__(self):
            self.burger=Burger()
        def addBuns(self, bunStyle):
            self.burger.setBuns(bunStyle)
            return self
        def addPatty(self, pattyStyle):
            self.burger.setPatty(pattyStyle)
            return self
        def addCheese(self, cheeseStyle):
            self.burger.setCheese(pattyStyle)
            return self
        def build(self):
            return self.burger
    burger=BurgerBuilder() \
            .addBuns('sesame') \
            .addPatty('fish patty') \
            .addCheese('swiss') \
            .build()
```

### Singelton
![Singleton](Images/singleton.png)
```py
    '''
    Singleton is a class that can only have a single instance
    Several use cases - maintaining a single copy of app's state
    '''
    #start with static instance var
    # want to know if user is logged in or not
    # don't use constructor to instantiate app state, use a static method that obtains app state

    class ApplicationState:
        instance=None
        def __init__(self):
            self.isLoggedIn=False
        @staticmethod
        def getAppState():
            # if there is not existing state, instantiate one, else return
            if not ApplicationState.instance:
                ApplicationState.instance=ApplicationState()
            return ApplicationState.instance
        appState1=ApplicationState.getAppState()
        print(appState1.isLoggedIn)#False
        #get app state again, is first instance
        #now returns True due to return
        appState2=ApplicationState.getAppState()
        #modify first app instance
        appState1.isLoggedIn=True
        print(appState1.isLoggedIn) #True
        print(appState2.isLoggedIn) #True

    '''
    Pattern is useful for multiple comps
    now having a shared source of truth
    '''
```

## Behavioral Patterns
### Observer (Pub Sub Pattern)
__How can all components of an app listen for updates in real time__
- Widely used pattern especially in Distributed Systems
- Subject/Pub is Youtube Channel with events being new video being uploaded and Observers/Subs
```py
    class YoutubeChannel:
        def __init__(self, name):
            self.name=name
            self.subscribers=[]
        def sub(self, sub):
            self.subscribers.append(sub)
        def notify(self, event):
            for sub in self.subscribers:
                sub.sendNotification(self.name, event)

    #defined sub interface def w/ abstractmethod/interface
    from abc import ABC, abstractmethod
    class YoutubeSubscriber(ABC):
        @abstractmethod
        def sendNotification(self, event):
            pass

    #Passes in YoutubeSubscriber in as a parameter
    class YoutubeUser(YoutubeSubscriber):
        def __init__(self, name):
            self.name=name
        def sendNotification(self, channel, event):
            print(f'User{self.name} received update from {channel}:{event}')

    channel = YoutubeChannel('neetcode')
    channel.subscribe(YoutubeUser('sub1'))
    channel.subscribe(YoutubeUser('sub2'))
    channel.subscribe(YoutubeUser('sub3'))
    #call notify once and all subs receive updates regarding subb'ed channels
    channel.notify('A New vid released')
```
### Iterator
- Simple pattern defines how values in object can be iterated through
```py
    '''
    myList=[1,2,3]
    for n in myList:
        print(n)
    '''
    #for complicated structures
    class ListNode:
        def __init__(self, val):
            self.val=val
            self.next=None
    class Linkedlist:
        def __init__(self,head):
            self.head=head
            self.cur=None
        #def iterator
        def __iter__(self):
            self.cur=self.head
            return self
        #iterate
        def __next__(self):
            if self.cur:
                val=self.cur.val
                self.cur=self.cur.next
                return val
            else:
                return StopIteration
    #Initialize
    head=ListNode(1)
    head.next=ListNode(2)
    head.next.next=ListNode(3)
    myList=LinkedList(head)
    #Iterate
    for n in myList:
        print(n)
```
### Strategy
- Want to modify or extend behavior of class without directly changing it
    - Open Closed Principle
        - Open: For extension, Closed: For modification
    - Ex: Filter array by removing negative values or removing odd

```py
    '''
    @abstractmethod decorator declares abstract methods in abstract base classes

    Abstract method = method declared in base class but does not provide an implementation
    Concrete subclass derived from base class must provide an implementation for abstract methods
        -  'TypeError' outputted at runtime if a subclass is not fully implementing abstract base class

    ABC - base class for creating abstract base
    abstractmethod - declares abstract methods
    any concrete subclass from 'FilterStrategy' must implement 'removeValue' method

    '''
    #define a filter strategy
    from abc import ABC, abstractmethod
    class FilterStrategy(ABC):
        @abstractmethod
        def removeValue(self, val):
            pass
    #create implementation that removes neg values
    class RemoveNegativeStrategy(FilterStrategy):
        def removeValue(self, val):
            return val < 0
    #create implementation that removes odd values
    class RemoveOddStrategy(FilterStrategy):
        def removeValue(self, val):
            return abs(val)%2
    #in runtime, pass strategy to values object
    class Values:
        def __init__(self, vals):
            self.vals=vals
        def filter(self, strategy):
            res=[]
            for n in self.vals:
                if not strategy.removeValue(n):
                    res.append(n)
            return res
    values=Values([-7, -4, -1, 0, 2, 6, 9])
    print(values.filter(RemoveNegativeStrategy()))
    #[0,2,6,9]
    print(values.filter(RemoveOddStrategy()))
    #[-4,0,2,6]
    # Can add additional strategies without modifying Values class
```
## Structural Patterns
### Adapter
- Analogous to screw that is too small to fit into hole, adapter makes screw compatible
```py
class UsbCable:
    def __init__(self):
        self.isPlugged=False
    def plugUsb(self):
        self.isPlugged=True
    class UsbPort:
        def __init__(self):
            self.portAvailable=True
        def plug(self, usb):
            if self.portAvailable:
                usb.plugUsb()
                self.portAvailable=False
    #usbCables can plug directly into usb ports
    usbCable=UsbCable()
    usbPort1=UsbPort()
    usbPort1.plug(usbCable)

class MicroUsbCable:
    def __init__(self):
        self.isPlugged=False
    def plugMicroUsb(self):
        self.isPlugged=True
class MicroToUsbAdapter(UsbCable):
    def __init__(self, microUsbCable):
        self.microUsbCable=microUsbCable
        self.microUsbCable.plugMicroUsb()
    #can override usbcable.plugusb() if needed

    #plug microusb cable into adapter into port
    #works like regular usb
    microToUsbAdapter=MicroToUsbAdapter(MicroUsbCable())
    usbPort2=UsbPort()
    usbPort2.plug(microToUsbAdapter())
```
### Facade
- Outward appearance = class or appearance working with as programmers, and less pleasant reality/complexity that is hidden
- Common Examples
    - HTTP Clients
        -
        ```js
            //basic fetch request
            //
            fetch('http:/example.com')
                .then((response)=> response.json())
                .then((data)=> console.log(data))
        /*
            someAsyncOperation()
                .then(result => {
                    // Handle the successful result
                })
                .catch(error => {
                    // Handle the error
                })
                .finally(() => {
                    // Cleanup or finalization tasks
                });
        */
        ```
    - Dynamic Arrays like Vectors in C++, ArrayList
        ```py
            #Py arrs are dynamic default, example of resizing
            class Array:
                def __init__(self):
                    self.capacity=2
                    self.length=0
                    self.arr=[0]*2
                def pushback(self, n):
                    if self.length==self.capacity:
                        self.resize()
                    self.arr[self.length]=n
                    self.length+=1
                def resize(self):
                    self.capacity=2*self.capacity
                    newArr=[0]*self.capacity
        ```
