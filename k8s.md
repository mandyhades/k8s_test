```
 _                             _  __     _                          _
| |                           | |/ /    | |                        | |
| |     ___  __ _ _ __ _ __   | ' /_   _| |__   ___ _ __ _ __   ___| |_ ___  ___
| |    / _ \/ _` | '__| '_ \  |  <| | | | '_ \ / _ \ '__| '_ \ / _ \ __/ _ \/ __|
| |___|  __/ (_| | |  | | | | | . \ |_| | |_) |  __/ |  | | | |  __/ ||  __/\__ \
|______\___|\__,_|_|  |_| |_| |_|\_\__,_|_.__/ \___|_|  |_| |_|\___|\__\___||___/
```

#---------------------------------------------------------------------------------------------------

# Note: This file is not used. Do not type in your solution in this file.

# Use the adjacent .yml or .txt file instead

# k8s : pods with YAML , Replicasets , Deployments , Deployments - Update and Rollback

# k8s Deployment: creat ,get ,update ,status ,rollback

---

## **_k8s architucture_**

## k8s architucture:

1 server Master Nodes , Multi Server Nodes

### k8s API

như giao diện người dùng cho Kubernetes. Người dùng, thiết bị quản lý, giao diện dòng lệnh, tất cả đều nói chuyện với máy chủ API để tương tác với Kubernetes cluster.

### k8s scheduler

Bộ lập lịch chịu trách nhiệm phân phối công việc hoặc vùng chứa trên nhiều nút.Nó tìm kiếm các vùng chứa mới được tạo và gán chúng cho các nút.

### k8s controller

Bộ điều khiển là bộ não đằng sau sự điều phối.Họ có trách nhiệm thông báo và phản hồi khi các nút, vùng chứa hoặc điểm cuối gặp sự cố.
Các bộ điều khiển đưa ra quyết định đưa ra các thùng chứa mới trong những trường hợp như vậy.

### k8s etcd

là một kho lưu trữ giá trị khóa đáng tin cậy phân tán được Kubernetes sử dụng để lưu trữ tất cả dữ liệu được sử dụng để quản lý cụm.
Nghĩ theo cách này.Khi bạn có nhiều nút và nhiều Master trong cụm của mình, etcd lưu trữ tất cả thông tin đó trên tất cả các nút trong cụm theo cách phân tán. etcd chịu trách nhiệm thực hiện các khóa trong cụm để đảm bảo rằng không có xung đột giữa các Master.

---

**_Master Nodes_**

---

### kube-apiserver (master node)

Máy chủ chính có máy chủ API kube và đó là những gì làm cho nó trở thành máy chủ Tương tự, các nút công nhân có tác nhân kubelet chịu trách nhiệm tương tác với một nút chính để cung cấp thông tin về tình trạng của nút công nhân và thực hiện các hành động do Chủ yêu cầu trên
các nút công nhân.

### etcd (master node)

Tất cả các thông tin thu thập được được lưu trữ trong một kho giá trị quan trọng trên bản gốc. Kho giá trị quan trọng dựa trên khuôn khổ etcd phổ biến như chúng ta vừa thảo luận.

### controller

### scheduler

---

**_Worker Nodes (minion or node)_**

---

### k8s kubelet (worker node)

Và cuối cùng Kubelet là tác nhân chạy trên từng nút trong cluster.Tác nhân có trách nhiệm đảm bảo rằng các vùng chứa đang chạy trên các nút như mong đợi.

### k8s kube-proxy (worker node)

chiu trach nhiem ve networking

---

**_pod_**

---

## pod

Như chúng ta đã thảo luận trước đây với Kubernetes, mục đích cuối cùng của chúng ta là triển khai ứng dụng của mình ở dạng container trên một tập hợp các máy được định cấu hình là các nút worker trong một cụm.Tuy nhiên, Kubernetes không triển khai các thùng chứa trực tiếp trên các nút worker.Các thùng chứa được gói gọn trong một đối tượng Kubernetes được gọi là Pod.Pod là một phiên bản duy nhất của một ứng dụng.Pod là đối tượng nhỏ nhất mà bạn có thể tạo trong Kubernetes.

Ở đây, chúng ta thấy trường hợp đơn giản nhất trong số các trường hợp đơn giản nhất khi bạn có một cụm Kubernetes một nút với một phiên bản ứng dụng duy nhất của bạn đang chạy trong một bộ chứa Docker duy nhất được gói gọn trong một Pod.

Điều gì sẽ xảy ra nếu số lượng người dùng truy cập ứng dụng của bạn tăng lên và bạn cần mở rộng quy mô ứng dụng của mình? Bạn cần thêm các phiên bản bổ sung của ứng dụng web của mình để chia sẻ tải. Bây giờ, bạn sẽ tạo ra các phiên bản bổ sung ở đâu? Chúng tôi có hiển thị phiên bản bộ chứa mới trong cùng một nhóm không? KHÔNG.

Chúng tôi tạo Pod mới hoàn toàn với một phiên bản mới của cùng một ứng dụng.Như bạn có thể thấy, hiện tại chúng ta có hai phiên bản ứng dụng web đang chạy trên hai nhóm riêng biệt trên cùng một hệ thống Kubernetes hoặc Node.Điều gì sẽ xảy ra nếu cơ sở người dùng tăng thêm và nút hiện tại của bạn không có đủ dung lượng?Vậy thì bạn luôn có thể triển khai các Pod bổ sung trên một nút mới trong cụm.

Bạn sẽ có một nút mới được thêm vào cụm để mở rộng khả năng vật lý của cụm.Vì vậy, điều tôi đang cố gắng minh họa trong trang trình bày này là các nhóm thường có mối quan hệ 1-1 với các vùng chứa đang chạy ứng dụng của bạn để mở rộng quy mô, bạn tạo các Pod mới và để giảm quy mô, bạn xóa các Pod hiện có.

Bạn không thêm các container bổ sung vào Pod hiện có để mở rộng quy mô ứng dụng của mình.Ngoài ra, nếu bạn đang thắc mắc về cách chúng tôi triển khai tất cả những điều này và cách chúng tôi đạt được cân bằng tảigiữa các vùng chứa, v.v.Chúng ta sẽ đi vào tất cả những điều đó trong một bài giảng sau.

Hiện tại, chúng tôi chỉ đang cố gắng hiểu các khái niệm cơ bản.Chúng tôi vừa nói rằng các nhóm thường có mối quan hệ 1-1 với các container, nhưng chúng tôi có bị hạn chế chỉ có một container trong một Pod không? KHÔNG.

Một Pod có thể có nhiều container ngoại trừ thực tế là chúng thường không phải là nhiều container cùng loại.Như chúng ta đã thảo luận trong slide trước, nếu ý định của chúng ta là mở rộng ứng dụng của mình, thì chúngta sẽ cần tạo các Pod bổ sung.

Nhưng đôi khi bạn có thể gặp một tình huống trong đó bạn có một container trợ giúp có thể đang thực hiện một số loại tác vụ hỗ trợ cho ứng dụng web của chúng ta, chẳng hạn như xử lý dữ liệu do người dùng nhập, xử lý một tệp do người dùng tải lên.Vân vân.Và bạn muốn các Container trợ giúp này tồn tại cùng với Container ứng dụng của mình?Trong trường hợp đó, bạn có thể có cả hai Container này thuộc cùng một Pod để khi một Container ứng dụng mới được tạo, trình trợ giúp cũng được tạo.
Và khi nó chết, người trợ giúp cũng chết vì họ là một phần của cùng một Pod.Hai Container cũng có thể giao tiếp trực tiếp với nhau bằng cách coi nhau là máy chủ lưu trữ cục bộ vì chúng chia sẻ cùng một không gian mạng.Ngoài ra, họ cũng có thể dễ dàng chia sẻ cùng một không gian lưu trữ.

---

**Container Runtime**

---

cli:

```
kubectl run nginx --image=nginx
kubectl get pods
kubectl describe pod nginx
kubectl get pods -o wide

```

### Container Runtime (Docker)

Container Runtime là phần mềm cơ bản được sử dụng để chạy các vùng chứa.Trong trường hợp của chúng tôi, đó là Docker. Nhưng cũng có những lựa chọn khác.

---

**kubectl**

---

Và cuối cùng chúng ta cũng cần tìm hiểu một chút về một trong những tiện ích dòng lệnh được gọi là công cụ dòng lệnh kube hay kubectl hoặc kube control như nó còn được gọi là.Công cụ kubectl được sử dụng để triển khai và quản lý các ứng dụng trên một cụm Kubernetes. Để lấy thông tin cụm, để nhậntrạng thái của các nút khác trong cụm và quản lý nhiều thứ khác.
Lệnh kubectl run được sử dụng để triển khai một ứng dụng trên cụm.
`kubectl run hello-minikube`

Lệnh thông tin cụm kubectl được sử dụng để xem thông tin về cụm và lệnh kube ctl get node được sử dụng để liệt kê tất cả các phần nút của cụm
`kubectl cluster info`
`kubectl get node`

---

**yaml**

---

## Key Value Pair

```
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chicken
```

## Array/Lists

```
Fruits:
-   Orange
-   Apple
-   Banana
Vegetables:
-   Carrot
-   Cauliflower
-   Tomato
```

## Dictionary/Map

```
Banana:
    Calories: 105
    Fat: 0.4 g
    Carbs: 27 g
Grapes:
    Calories: 62
    Fat: 0.3 g
    Carbs: 16 g
```

## Key Value / Dictionary / Lists

```
Fruits:
    -    Banana:
             Calories: 105
             Fat: 0.4 g
             Carbs: 27 g
    -    Grape:
             Calories: 62
             Fat: 0.3 g
             Carbs: 16 g
```

## Dictionary vs List vs List of Dictionaries

### Dictionary

```
Color: Blue
Model: Corvette
Transmission: Manual
Price: $20,000
```

### Dictionary in Dictionary:

```
Color: Blue
Model:
    Name: Corvette
    Year: 1995
Transmission: Manual
Price: $20,000
```

### List of Strings

- Blue Corvette
- Grey Corvette
- Red Corvette
- Green Corvette
- Blue Corvette
- Black Corvette

### List of Dictionaries

```
- Color: Blue
  Model:
      Name: Corvette
      Year: 1995
  Transmission: Manual
  Price: $20,000
- Color: Grey
  Model:
      Name: Corvette
      Year: 1995
  Transmission: Manual
  Price: $20,000
- Color: Red
  Model:
      Name: Corvette
      Year: 1995
  Transmission: Manual
  Price: $20,000
- Color: Green
  Model:
      Name: Corvette
      Year: 1995
  Transmission: Manual
  Price: $20,000
- Color: Blue
  Model:
      Name: Corvette
      Year: 1995
  Transmission: Manual
  Price: $20,000
- Color: Black
  Model:
      Name: Corvette
      Year: 1995
  Transmission: Manual
  Price: $20,000
```

---

**yaml note**

---

- Dictionary - Unordered
- List - Ordered

## Dictionary/Map - hai Dictionary ben duoi la bang nhau :

```
Banana:
    Calories: 105
    Carbs: 27 g
    Fat: 0.4 g

Banana:
    Calories: 105
    Fat: 0.4 g
    Carbs: 27 g
```

## Array/List - hai Array ben duoi la khac nhau :

```
Fruits:
-   Orange
-   Apple
-   Banana
Fruits:
-   Orange
-   Banana
-   Apple
```

## ghi chu trong Yaml:

ghi chu bang ky tu "#":

```
# List of Fruits:
Fruits:
-   Orange
-   Apple
-   Banana
```

```
Employee:
  Name: Jacob
  Sex: Male
  Age: 30
  Title: Systems Engineer
  Projects:
    - Automation
    - Support
  Payslips:
      - Month: June
        Wage: 4000
      - Month: July
        Wage: 4500
      - Month: August
        Wage: 4000
```

## dry run k8s:

kubectl get pods
kubectl run nginx --image nginx
kubectl describe pod nginx
kubectl delete pods nginx
kubectl run redis --image=redis123 --dry-run=client -o yaml

kubectl run redis --image=redis123 --dry-run=client -o yaml > redis.yaml
kubectl create -f redis.yaml
vim redis.yaml
kubectl apply -f redis.yaml

## replication controller vs replica set

replication controller : old , sap bi loai bo
replica set : new , duoc su dung rong rai

## **_k8s networking_**
- minikube là một máy ảo . vd: host: 192.168.1.10 , minikube:192.168.1.5
- worker node tạo ra các pod , các pod được tạo ra là ip private (có dạng 10.244.0.2,10.244.0.3 ...)

## cluster networking k8s
sử dụng flanel : https://github.com/flannel-io/flannel
sử dụng Calico (khuyến nghị) : https://www.tigera.io/project-calico/
https://www.kube-router.io/
https://en.wikipedia.org/wiki/OSI_model
https://kubernetes.io/docs/concepts/cluster-administration/networking/


## **_k8s service network_**

có 3 loại service network:
## **NodePort**
Ánh xạ ip:port POD - ip:port service - ip:port NodePort (ip: ip worker node,range port: 30000-32767)
## **Cluster IP**
cluster 3 node fron end hoặc 3 node backend hoặc 3 node db cache (redis)
## **Load Balancer**
1 domain nhiều ip ( load balance giống nginx)
