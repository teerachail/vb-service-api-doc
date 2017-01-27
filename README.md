## Virtual Bank Service API Documentation
---

### Overview
> การเชื่อมต่อระบบ VirtualBank ผ่าน Open Service API ทำให้เกืดความยืดหยุ่นในการออกแบบระบบของ 3rd party ที่ต้องการเข้าใช้งานร่วมกับระบบ VirtualBank

### Open Service API
> Open Service API คือรูปแบบการติดต่อเพื่อทำงานร่วมกับระบบใดๆ ในรูปแบบที่ผู้พัฒนาออกแบบไว้ โดยอาศัย Open Web Standard ในการติดต่อสื่อสาร โดยปกติมักจะใช้ HTTP สร้างระบบที่มีคุณสมบัติแบบ HATEOS


#### ตัวอย่าง

VirtualBank provides various payment-related operations through the `/payment` resource and related sub-resources. 
Use payment for direct payments and payments for any purchases. You can also use sub-resources to get payment-related details.

** Create a payment **
```
POST:

/v1/payments/payment
```
