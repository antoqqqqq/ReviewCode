# Hướng dẫn kết nối SonarCloud với Sonar IDE và MCP

## 1. Chuẩn bị SonarCloud

1. Tạo tài khoản hoặc đăng nhập SonarCloud: [https://sonarcloud.io](https://sonarcloud.io)
2. Lấy các thông tin cần thiết:

   * **Organization Key** (SONARQUBE_ORG)
   * **Token truy cập** (SONARQUBE_TOKEN)

Ví dụ:

```text
SONARQUBE_ORG = "antoqqqqq"
SONARQUBE_TOKEN = "146fb3cdf98e88888065a9ddedfff9f86918c703"
```

---

## 2. Cài đặt Sonar IDE

1. Tải Sonar IDE từ MCP (hoặc SonarCloud IDE) và cài đặt theo hướng dẫn.
2. Mở Sonar IDE và đảm bảo cổng kết nối với MCP trống, ví dụ: **64120**

---

## 3. Cấu hình MCP (`mcp.json`)

Tạo file `mcp.json` trong project của bạn với nội dung:

```json
{
  "servers": {
    "sonarqube": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "SONARQUBE_TOKEN",
        "-e",
        "SONARQUBE_ORG",
        "-e",
        "SONARQUBE_IDE_PORT",
        "mcp/sonarqube"
      ],
      "env": {
        "SONARQUBE_ORG": "antoqqqqq",
        "SONARQUBE_TOKEN": "146fb3cdf98e88888065a9ddedfff9f86918c703",
        "SONARQUBE_IDE_PORT": "64120"
      }
    }
  }
}
```

**Giải thích cấu hình:**

* `command: "docker"` → MCP sẽ chạy SonarQube bằng Docker.
* `args` → Các tham số khởi chạy Docker.
* `env` → Thiết lập biến môi trường quan trọng:

  * `SONARQUBE_ORG`: tổ chức của bạn trên SonarCloud
  * `SONARQUBE_TOKEN`: token bảo mật
  * `SONARQUBE_IDE_PORT`: cổng IDE để MCP kết nối

---

## 4. Chạy MCP với Sonar IDE

1. Mở terminal tại thư mục chứa `mcp.json`
2. Chạy lệnh MCP (tùy hệ thống):

```bash
mcp start
```

3. Sonar IDE sẽ tự động kết nối đến SonarCloud thông qua MCP.

---

## 5. Kiểm tra kết nối

* Mở Sonar IDE, nếu IDE hiển thị project và phân tích code bình thường, kết nối đã thành công.
* Nếu gặp lỗi cổng hoặc token, kiểm tra lại `mcp.json` và token trên SonarCloud.
