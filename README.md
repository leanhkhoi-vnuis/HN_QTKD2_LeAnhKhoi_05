-- Phần I : THIẾT KẾ VÀ KHỞI TẠO CSDL (20 điểm)
-- Câu 1. Tạo CSDL và các bảng 

-- Viết lệnh tạo một CSDL mới tên là AirlineManagement và sử dụng nó. (5 điểm)
create database AirlineManagement01;
use AirlineManagement01;

--  tạo 3 bảng dưới đây với đầy đủ các cột 
create table Passengers(
PassengerId varchar(10) primary key,
FullName varchar(100) not null unique,
PassportNumber varchar(20) unique,
Nationality varchar(50) default('Viet Nam')
);

create table Flights(
FlightId varchar(10) primary key,
DepartureCity varchar(50) not null,
ArrivalCity varchar(50) not null,
FlightDate date,
FlightTime time
);

create table Tickets(
PassengerId varchar(10),
FlightId varchar(10),
foreign key (PassengerId) references Passengers(PassengerId),
foreign key (FlightId) references Flights(FlightId),
primary key (PassengerId, FlightId),
Class enum('PhoThong', 'ThuongGia') default('PhoThong'),
Price decimal(15, 2)
);

-- Phần II: Thao tác dữ liệu
-- Câu 2: Thêm dữ liệu (15 điểm)

insert into Passengers
values ('HK01', 'Nguyen Van Hung', 'B1234567', 'Viet Nam'),
('HK02', 'John Smith', 'C9876543', 'USA'),
('HK03', 'Tran Thi Mai', 'B2345678', 'Viet Nam'),
('HK04', 'Lee Min Ho', 'K1122334', 'Korea'),
('HK05', 'Pham Van Tuan', 'B3456789', 'Viet Nam');

insert into Flights
values ('VN100', 'Ha Noi', 'Da Nang', '2023-10-20', '08:00:00'),
('VN200', 'Ho Chi Minh', 'Ha Noi', '2023-10-21', '14:30:00'),
('VJ300', 'Ha Noi', 'Tokyo', '2023-10-22', '23:00:00');

insert into Tickets
values ('HK01', 'VN100', 'PhoThong', 1500000),
('HK01', 'VN200', 'ThuongGia', 3000000),
('HK02', 'VN100', 'PhoThong', 1500000),
('HK03', 'VN200', 'PhoThong', 1200000),
('HK03', 'VJ300', 'PhoThong', 5000000),
('HK04', 'VJ300', 'ThuongGia', 10000000),
('HK05', 'VN100', 'PhoThong', 1400000);

-- Câu 3: Cập nhật dữ liệu (10 điểm)
-- Cập nhật giờ bay (FlightTime) của chuyến bay 'VN100' thành '09:00:00'.
update Flights
set FlightTime =  '09:00:00'
where FlightId = 'VN100';

-- Tăng giá vé (Price) thêm 500000 cho khách hàng 'HK04' trên chuyến bay 'VJ300'.
update Tickets
set Price = 10500000
where PassengerId = 'HK04' and FlightId = 'VJ300'; 

-- Hủy vé máy bay của hành khách 'HK02' trên chuyến bay 'VN100'.
delete from Tickets
where PassengerId = 'HK02' and FlightId = 'VN100';

-- PHẦN 3: TRUY VẤN DỮ LIỆU - (50 điểm)
-- Câu 4: Truy vấn cơ bản (30 điểm)

 -- Lấy danh sách hành khách (PassengerId, FullName) có quốc tịch là 'Viet Nam'.
select PassengerId, FullName 
from Passengers
where Nationality = 'Viet Nam';

-- Lấy danh sách các chuyến bay (FlightId, ArrivalCity) bay đến 'Ha Noi'.
select FlightId, ArrivalCity
from Flights
where ArrivalCity = 'Ha Noi';

-- Lấy danh sách vé máy bay (PassengerId, FlightId, Price) có hạng vé là 'ThuongGia'.
select PassengerId, FlightId, Price
from Tickets
where Class = 'ThuongGia';

-- Tìm các chuyến bay có ngày bay (FlightDate) trước ngày '2023-10-22'.
select * from Flights
where FlightDate <  '2023-10-22';

-- Lấy ra danh sách vé máy bay, sắp xếp theo giá giảm dần.
select * from Tickets
order by Price desc;

-- Lấy ra thông tin của 2 hành khách đầu tiên trong bảng Passengers.
select * from Passengers
order by FullName desc
limit  2;

-- Thống kê tổng số tiền bán vé thu được từ mỗi chuyến bay (FlightId) dựa trên bảng Tickets.
select 
   FlightId,
    SUM(Price) as TotalRevenue
from Tickets
group by FlightId;

-- Đếm số lượng hành khách đã đặt vé cho từng hạng vé (Class). Hiển thị: Class, Amount.
select 
    Class,
    COUNT(*) as Amount
from Tickets
group by Class;

-- Tìm những chuyến bay (FlightId) có ít nhất 3 hành khách đã đặt vé.
select 
    FlightId
from Tickets
group by FlightId
having COUNT(*) >= 3;




