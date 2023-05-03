**Đây là các bài liên quan đến các tập file PCAP trong PicoCTF.** 

## 1. Wireshark doo doo do doo...

### Đề bài

![](Up%20Challenge/wiresharkdoo0.png)!

### Cách giải

<p> Đầu tiên ta sử dụng phần mềm Wireshark để mở file này lên <p>

![](Up%20Challenge/wiresharkdoo1.png)

<p> Ta thấy ở phần Protocol (giao thức) có 2 loại giao thức là HTTP và TCP, thế nên ta ấn Analyze chọn Follow và chọn TCP stream để kiểm tra các gói tin chuyển đi. Ở stream thứ 5 ta nhận thấy có đoạn mã bất thường. <p>

![](Up%20Challenge/wiresharkdoo2.png)

<p> Copy và dịch đoạn mã trên (Mật mã Caesar) ta được flag <p>

```Flag: picoCTF{p33kab00_1_s33_u_deadbeef}```

## 2.Wireshark twoo twooo two twoo...

### Đề bài 

![](Up%20Challenge/wiresharktwo0.png)

### Cách giải

<p> Mở bài này trong wireshark, ta nhận thấy tập tin này chứa rất nhiều giao thức. Nhưng vẫn nên kiểm tra bằng cách follow và chọn TCP stream (HTTP stream) để xem dữ liệu chuyển đi có chứa flag hay không. Ở stream thứ 6 ta nhận thấy có flag, nhưng rất đáng nghi <p>

![](Up%20Challenge/wiresharktwo1.png)

<p> Tìm đến các stream tiếp theo, ta nhận thấy có nhiều các flag như vậy nữa hiện ra, vậy nên ta biết được đây chính là flag fake, làm lệch hướng suy luận của mình <p>

<p> Kiểm tra lướt qua các tập tin, ta thấy ở những địa chỉ DNS (địa chỉ ở cột source là 8.8.8.8) luôn có 1 đoạn mã đáng ngờ <p>

![](Up%20Challenge/wiresharktwo2.png)

 Ta sử dụng lệnh lọc filter để lấy những stream ở địa chỉ DNS đó (lệnh filter **dns && ip.src ==8.8.8.8**) nhưng ta thấy được nó gửi đến địa chỉ 192.168.38.104 và khi đổi địa chỉ ở câu lệnh trên thành 192.168.38.104, ta lại thấy nó gửi đến 1 địa chỉ mới nữa là 18.217.1.57.

![](Up%20Challenge/wiresharktwo3.png)

Có thể có gì đó đáng ngờ ở đây, ta thử đổi câu lệnh thành **dns && ip.dst ==18.217.1.57** thì nhận được thông tin ngắn gọn hơn hẳn những đường dẫn trên

![](Up%20Challenge/wiresharktwo4.png)

<p> Ghép các mảnh của đoạn mã lại và decode base64 ta được flag <p>

``` Flag: picoCTF{dns_3xf1l_ftw_deadbeef}```

## 3. Packets Primer

### Đề bài 

![](Up%20Challenge/packet0.png)

### Cách giải

<p> Bài này tương tự như bài Wireshark doo doo do doo..., chúng ta chỉ cần ấn Analyze, chọn Follow và chọn vào giao thức của file <p>

``` Flag: picoCTF{p4ck37_5h4rk_ceccaa7f} ```

## 4. Pcapoisoning

### Đề bài

![](Up%20Challenge/poison0.png)

### Cách giải

<p> Đề cho ta 1 file có rất nhiều tập tin, nhưng hầu như trong đó không có nhiều ý nghĩa. Bài này có 100 điểm nên suy nghĩ đơn giản, thử sử dụng lọc filter xem thế nào. <p>

<p> Vì trong file hầu như sử dụng giao thức TCP nên ta sử dụng câu lệnh tcp contains "pico" <p>

![](Up%20Challenge/poison1.png)

<p> Thấy ngay flag trong dữ liệu <p>

``` Flag: picoCTF{P64P_4N4L7S1S_SU55355FUL_0f2d7dc9} ```

## 5. Trivial Flag Transfer Protocol

### Đề bài 

![](Up%20Challenge/trivial0.png)

### Cách giải 

<p> Bài này khi mở file ta thấy giao thức sử dụng là TFTP là một giao thức truyền tệp tin trực tiếp (không nén file hoặc tách file thành các frame và packet) <p>
<p> Vì là truyền file trực tiếp nên ta có thể lọc luôn file ra bằng cách ấn File chọn Export Object chọn giao thức TFTP, nó sẽ hiện ra các file được gửi đi <p>

![](Up%20Challenge/trivial1.png)

<p> Kiểm tra từng file xem file nào có flag thôi. Thấy ngay file txt chứa 1 đoạn mã đáng ngờ, copy và kiểm tra, ta biết được đây là đoạn mã Rot13. Dịch thì không ra flag nhưng ra dữ liệu (hơi dính 1 tí nên cần kiến thức tiếng anh để cắt và dịch câu) <p>

![](Up%20Challenge/trivial2.png)

<p> Nó bảo là hãy giấu flag và tôi sẽ check lại cho cái plan và đồng thời lại có 1 file tên là plan. Mở nó lên, lại là Rot13 và dịch ra <p>

![](Up%20Challenge/trivial3.png)

<p> Giờ thì nó bắt xem ảnh, oke lại ngồi kiểm tra các ảnh, mở ra thì không thấy có gì lạ thường, các file ảnh đều dưới dạng bmp <p>
<p> Khả năng là dữ liệu bị khắc trong ảnh bằng kỹ thuật stegano. Nhưng với dạng file bmp thì ta không thể sử dụng zsteg, exiftool được. Buộc ta phải có 1 tool khác tên steghide <p> 
<p> Cho đống ảnh này vào máy ảo và làm việc thôi <p>
<p> Bất ngờ ở đây, khi extract (giải nén) file, nó bắt ta phải nhập 1 passphrase tức mật khẩu <p>

![](Up%20Challenge/trivial4.png)

<p> Mật khẩu ở đâu??? À, ta nhận ra ở file Plan nó đã bảo "I USED THE PROGRAM AND HID IT WITH-DUEDILIGENCE" có lẽ mật khẩu là DUEDILIGENCE chăng? <p>

<p> Sử dụng cùng với 1 câu lệnh khác, ta extract các file bmp được cho <p>

![](Up%20Challenge/trivial5.png)

<p> Và ta nhận được 1 file là flag.txt khi giải nén ảnh thứ 3. Mở file và nhận được flag <p>

``` Flag: picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919} ```

## 6. Shark on wire

### Đề bài 

![](Up%20Challenge/shark1_0.png)

### Cách giải 

<p> Nó có khá nhiều các giao thức lạ, nhưng mà vẫn xuất hiện các giao thức UDP và TCP, chọn analyze, follow và ấn UDP (TCP) stream thôi <p>

<p> Thấy ngay flag ở stream thứ 6 <p> 

``` Flag: picoCTF{StaT31355_636f6e6e} ```

## 7. Find and Open

### Đề bài 

![](Up%20Challenge/find0.png)

### Cách giải 

<p> Đề cho ta 1 file pcap và 1 file zip, đề bắt ta phải mở được file zip thì mới lấy được flag, vậy nên ta phải tìm thông tin flag trong file pcap thôi <p>

<p> File có vẻ khá ngắn nên ta có thể check bằng tay các packet được gửi đi và nhận về <p>

![](Up%20Challenge/find1.png)

<p> Ta tìm được 1 đoạn tin rất khả nghi, vì nó sử dụng 1 giao thức khác hẳn so với các giao thức được cho ở trên, đồng thời đoạn thông tin mà nó gửi đi có dấu hiệu của 1 đoạn mật mã được mã hóa <p> 

![](Up%20Challenge/find2.png)

 Giải mã đoạn mã trên ta được 1 nửa của flag ``` This is the secret: picoCTF{R34DING_LOKd_ ```

![](Up%20Challenge/find3.png)

<p> Nhưng đây mới chỉ là 1 nửa thôi, nửa còn lại chắc chắn đang ở trong file zip, đọc lại phần Hint thì nó có nói <p>  

![](Up%20Challenge/find4.png)

<p> Vậy có lẽ nào mật khẩu chính là nửa đoạn mã mình vừa tìm được. Thử ngay <p> 

![](Up%20Challenge/find5.png)

<p> Và ta nhận được luôn flag <p> 

![](Up%20Challenge/find6.png)

``` Flag: picoCTF{R34DING_LOKd_fil56_succ3ss_0f2afb1a} ```

## 8. WPA-ing Out

### Đề bài

![](Up%20Challenge/wpa0.png)

### Cách giải 

<p> Đề bài đã cho ta khá nhiều dữ liệu quan trọng: <p>
<p> * Thứ nhất : Mật khẩu này có thể crack được <P>
<p> * Thứ hai : Mật khẩu sử dụng bảo mật của Wireless (WPA) <p>
<p> * Thứ ba : Mật khẩu có thể có trên rockyou wordlist <p>
<p> * Thứ tư : Tool có thể sử dụng là aircrack-ng (ở trong hint 2) <p>
<p> Vậy thì ta sẽ down tool về ubuntu, sử dụng bộ mật khẩu trong rockyou wordlist với câu lệnh sau <p>

![](Up%20Challenge/wpa1.png)

<p> Tool sẽ trả lại các kết quả <p>

![](Up%20Challenge/wpa2.png)

<p> Đề có nói là tìm mật khẩu nên có thể Key tìm được trong wordlist có thể trùng với Flag, thử nhập kết quả ta có luôn flag <p>

``` Flag: picoCTF{mickeymouse} ```

## 9. Eavesdrop

### Đề bài 

![](Up%20Challenge/eave0.png)

### Cách giải
