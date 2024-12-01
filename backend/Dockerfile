# Sử dụng Gradle chính thức để build ứng dụng
FROM gradle:8.3-jdk17 AS builder

# Thiết lập thư mục làm việc
WORKDIR /app

# Sao chép file build.gradle và settings.gradle (nếu có)
COPY build.gradle settings.gradle ./

# Tải dependencies để cache
RUN gradle build --info --no-daemon --stacktrace || return 0

# Sao chép mã nguồn vào container
COPY . . 

# Build ứng dụng
RUN gradle clean build -x test --info --no-daemon --stacktrace

# Sử dụng OpenJDK nhẹ để chạy ứng dụng
FROM openjdk:17-jdk-slim

# Thiết lập thư mục làm việc
WORKDIR /app

# Sao chép file JAR từ stage trước
COPY --from=builder /app/build/libs/*.jar app.jar

# Expose cổng 8080 (mặc định cho Spring Boot)
EXPOSE 8080

# Chạy ứng dụng
ENTRYPOINT ["java", "-jar", "app.jar"]
