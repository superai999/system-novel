Để tạo một container `Docker` bằng `Docker API` trong `Node.js`, bạn có thể sử dụng thư viện `axios` hoặc `node-fetch` để gửi các yêu cầu HTTP đến `Docker API`.

Dưới đây là một ví dụ sử dụng thư viện `axios` để tạo một container Python bằng `Docker API` trong `Node.js`:

1. Cài đặt thư viện `axios`: `npm install axios`

2. Sử dụng code sau để tạo container:

```js
const axios = require('axios');

// Thông tin của container cần tạo
const containerInfo = {
  Image: 'python:latest',
  Cmd: ['/bin/bash'],
  AttachStdin: false,
  AttachStdout: true,
  AttachStderr: true,
  Tty: true,
  OpenStdin: false,
  StdinOnce: false,
  HostConfig: {
    AutoRemove: true, // Container tự động xóa sau khi dừng
  },
};

// Tạo container thông qua Docker API
axios.post('http://localhost:2375/containers/create', containerInfo)
  .then(response => {
    const containerId = response.data.Id;
    console.log(`Container ID: ${containerId}`);

    // Khởi động container
    axios.post(`http://localhost:2375/containers/${containerId}/start`)
      .then(() => {
        console.log('Container started.');
      })
      .catch(error => {
        console.error('Error starting container:', error);
      });
  })
  .catch(error => {
    console.error('Error creating container:', error);
  });
```

Điều quan trọng là địa chỉ URL `http://localhost:2375` có thể thay đổi tùy thuộc vào cài đặt `Docker` của bạn. Chắc chắn rằng `Docker daemon` đang chạy và API của nó có thể được truy cập từ `Node.js`.

Lưu ý: Việc truy cập `Docker daemon` qua HTTP API có thể có rủi ro bảo mật. Đảm bảo rằng bạn đã cấu hình `Docker daemon` của mình một cách an toàn và cân nhắc về bảo mật khi sử dụng API này.











