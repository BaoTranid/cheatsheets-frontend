# Cheatsheets FrontEnd

---

## title: HTML

> Khi viết html, đặc biệt là form input, đừng quên các attributes bổ trợ để tăng UX. Ví dụ với một form input nhận credit card. Thay vì viết

```html
<label>Card Number</label> <input type="text" />
```

Hãy viết

```html
<label for="frmCCNum">Card Number</label>
<input
  type="text"
  id="frmCCNum"
  autocomplete="cc-number"
  inputmode="numeric"
  autocorrect="off"
  spellcheck="false"
  aria-label="Card number"
  placeholder="1234 1234"
/>
```

- Dùng labels kết hợp với id để tăng touch area, người dùng chỉ cần ấn vào labels hoặc input để focus vào input.
- Autocomplete để ... auto complete :shrug: nếu người dùng đã save credit card ở browser.
- Inputmode=“numeric” để chuyển về bàn phím số trên mobile.
- Tắt autocorrect và spellcheck vì ko cần thiết với credit card.
- Aria-label để phục vụ người dùng trên screen reader.
- Placeholder để hint về format.

---

## title: CSS

> Để css cho 1 thẻ A kề nó là thẻ B và chỉ khi đúng kề thẻ B thẻ A mới có style như mình định nghĩa.
> ví dụ: h1 kề h2 và h1 đứng 1 mình dùng (**:has**)

```html
<article>
  <h1>Morning Times</h1>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit</p>
</article>
<article>
  <h1>Morning Times</h1>
  <h2>Delivering you news every morning</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit</p>
</article>
```

```css
h1,
h2 {
  margin: 0 0 1rem 0;
}
h1:has(+ h2) {
  margin: 0 0 0.25rem 0;
}
```

---

## title: JS

> Khi làm web có dark mode và light mode thì khi load mới trang web cần biết được người dùng đang dùng loại mode nào để lấy nó hiển thị đầu tiên hoặc là user đã chọn rồi thì sẽ giữ lựa chọn đó.
> Để biết được người dùng đang chọn loại mode nào thì check biến **prefers-color-scheme** để lấy được loại mode mà người dùng đang dùng trên máy.

```js
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
    const newColorScheme = event.matches ? "dark" : "light";
});

# OR

if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
  // dark mode
}
```

> Khi gặp case: trong số STT cần thêm số 0 trước số có 1 chữ số VD: 01, 02, … , 11, 12 hoặc gặp case thêm dấu \* trước 1 dãy số VD thẻ visa: ****\*****0345.
> Thay vì viết:

```js
const getSTT = (number) => {
  if (number < 9) {
    return `0${number}`;
  }

  return number;
};

const getCard = (number, len = 11) => {
  return new Array(len - number.toString().length).fill("*").join("") + number;
};
```

Hãy viết:

```js
const getSTT = (number) => {
  return number.toString().padStart(2, "0");
};

const getCard = (number, len = 11) => {
  return number.toString().padStart(len, "*");
};
```

---

## title: React js

> Kĩ thuật memoized:
> kĩ thuật ghi nhớ input, output nếu input ko thay đổi trả về output cũ, nếu input thay đổi thực thi hành động để lấy output mới và trả về.
> Trong React thường được sử dụng như **useMemo**, **useCallback**. Về useCallback là 1 function truyền vào 1 array dependencies nếu 1 trong các biến này thay đổi thì function sẽ thực thi lại, nhằm biên dịch lại function trong useCallback.
> Nên sử dụng useCallback trong các trường hợp cần tối ưu hiệu suất và tránh việc render lại không cần thiết của các thành phần.
> Có 2 trường hợp hay dùng vs useCallback:

- Truyền 1 function cha sang con. Giúp Component con ko re-render lại khi Component cha render.
- Dùng useCallBack khi function nằm trong useEffect đòi hỏi phải luôn biện dịch code mới khi useEffect chạy.
  Kĩ thuật memoized dễ gây bug khi ae không kiễm soát tốt các dependencies.
  Nên sử dụng vào các event render nhiều như event trên canvas.

---

## title: GIT

> Soft skill: lấy lại code khi lỡ tay reset hard, xóa commit, xóa branch, …
> Dùng git reflog để khôi phục lại 1 commit bất kì

```bash
$ git reflog
0272a7e HEAD@{1}: reset: moving to HEAD~
4e33eb9 HEAD@{2}: commit: Add common components
0232a9e HEAD@{3}: commit: Add i18n config
b5d314a HEAD@{10}: commit: Add common utils
c528ae5 HEAD@{11}: pull: Fast-forward
```

Sau đó dùng `git reset --hard <commit>` để lấy lại commit vừa làm mất.

---

## title: HTTP

> Hiểu rõ về HTTP response status codes sẽ giúp làm việc với API, team BE thuận tiện hơn. Có 5 loại HTTP Status code.

- 1xx. Các status code loại này dùng để đơn giản thông báo với client rằng server đã nhận được request. Các status code này ít được sử dụng.
- 2xx. Các status code loại này có ý nghĩa rằng request được server và xử lý thành công.
- 3xx. Các status code loại này có ý nghĩa rằng server sẽ chuyển tiếp request hiện tại sang một request khác và client cần thực hiện việc gửi request tiếp theo đó để có thể hoàn tất. Thông thường khi trình duyệt nhận được status code loại này nó sẽ tự động thực hiện việc gửi request tiếp theo để lấy về kết quả.
- 4xx. Các status code loại này có ý nghĩa rằng đã có lỗi từ phía client trong khi gửi request. Ví dụ như sai URL, sai HTTP method, không có quyền truy cập vào trang…
- 5xx. Các status code loại này có ý nghĩa rằng đã có lỗi từ phía server trong khi xử lý request. Ví dụ như server quá tải, hết bộ nhớ, lỗi kết nối database…

---

## title: Package managers (npm, yarn or pnpm ...)

> Khi code thấy ổ đĩa mình đầy 1 cách vô cớ, vì gần đây không cài đặt thêm 1 app gì nặng cả.
> Hãy dùng lệnh `npm cache clean --force` và `yarn cache clean` để remove các package đang được cache nhất là yarn.
> Yarn cache các package đã được dùng rồi để khi gọi đến nó yarn lấy từ local đẩy vào Project sẽ nhanh hơn. Đâu đó sẽ có 20gb -> 40gb khoảng trống.

---

## title: Worker

> Worker là gì?
> Một BE nghe về worker sẽ nhiều hơn 1 FE. Nhưng mà FE cũng có worker :smile: nhưng mà worker của browser để AE sử dụng.
> AE đi phỏng vấn mà phỏng vấn technical. Mà Technical dùng thuật ngữ tiếng anh nhiều đôi khi AE sẽ ngợp vì có những thứ AE làm rồi nhưng mà thuật ngữ tiếng anh là gì AE sẽ không rõ.
> Trước khi nói về worker mình phải nói về ông thần javascript trước.

- Js là 1 ngôn ngữ chạy single thread (đơn luồng). Mọi tác vụ được đưa vào CallStack rồi lần lượt sử lý.
- Worker sinh ra để chạy song song với luồng xử lý hiện tại của dự án, xử lý các vấn đề phức tạp đòi hỏi độ trể lớn mà không ảnh hưởng đến luồng xử lý hiện tại của dự án đang chạy.
  Các Service workers của browser: Location, Notifications, Device Orientation

---

## title: Tương tác giữa js thuần và reactjs/vuejs

> Sử dụng.
> document.dispatchEvent và new CustomEvent
> ví dụ:

- document.dispatchEvent(new CustomEvent('randomNumberChanged', { detail: this.randomNumber })).
