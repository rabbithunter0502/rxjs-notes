Q: angular docs có ghi FormArray là để định nghĩa dynamic form, có thể thêm bớt controls ở run time.
<br/>Vậy nó khác gì so với dùng FormGroup, mình cũng có thể dùng addControl/removeControl để thêm bớt control tại runtime.
<br/>
<br/>
A: Việc dùng FormGroup hay FormArray phụ thuộc hoàn toàn vào 3 yếu tố:
- Bạn muốn giá trị Form của bạn có hình dạng như thế nào?
- Bạn muốn validate Form của bạn như thế nào?
- Bạn render form như thế nào?
Mình lấy ví dụ của Tien Nam Pham là danh sách users.
Nếu dùng FormGroup để quản lý nhiều user, bạn có 2 cách:
1. Dùng FormControl bên trong FormGroup để quản lý 1 Array<string>, là list user ids hoặc là list user names chẳng hạn vì FormControl rất cồng kềnh nếu dùng để quản lý object. Có thể sẽ bị hạn chế nếu theo cách tiếp cận này. Loại bỏ
2. Dùng FormGroup với nhiều FormGroup khác nhau, mỗi FormGroup là 1 user. Nhưng điều này cũng có nghĩa là dạng của giá trị form sẽ như sau:
{ user1: {...}, user2: {...}, user3: {...} }
có thể đây là điều bạn muốn nhưng trong trường hợp thêm thớt vào danh sách user thì có thể ko hợp lý.
Nếu dùng FormArray, thì bạn có thể quản lý như kiểu 1 array của các FormGroup, mỗi group là 1 user. Điều này giúp cho thêm thớt FormGroup cho mỗi user (FormArray quản lý) dễ dàng. Dễ validate vì mỗi user là 1 Group khác nhau, có thể validate riêng hoặc chung. Hình dạng giá trị Form cũng sẽ tường minh hơn:
{ user: [{...}, {...}] }
  
Q: Mọi người cho em hỏi làm thế nào để hiển thị page break effect như này ạ?
Tức là fetch html từ api về, html trong api dài và cần hiển thị page break ra như vậy thay vì trải dài từ trên xuống
Em có search mà nó toàn ra page break lúc print thôi, còn đây em cần hiển thị nó ra giao diện luôn ạ! [209965468_1866457470187613_6828520067611349444_n](https://user-images.githubusercontent.com/43747716/130489691-b95bc9dc-e316-40fc-a527-fd6c20a94db6.jpg)
