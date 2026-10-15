# คู่มือทำ Ansible Lab: Install Nginx

## 1. ข้อมูลนักศึกษา
- **รหัสนักศึกษา:** 674259006
- **ชื่อ-นามสกุล:** Nattaphon Klinjun

---

## 2. ไฟล์ที่ต้องมีในโฟลเดอร์ `/home/vboxuser/ansible-lab`

### ไฟล์ 1: `inventory.ini`
```ini
[webservers]
web01 ansible_host=192.168.56.11
web02 ansible_host=192.168.56.12

[webservers:vars]
ansible_user=vboxuser
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
*(หมายเหตุ: เช็ค IP ของ web01 และ web02 ในเครื่องของคุณว่าตรงกับ 192.168.56.11 หรือไม่)*

---

### ไฟล์ 2: `install-nginx.yml`
*(ชื่อ Play และ Task ตรงกับภาพของอาจารย์เป๊ะๆ 4 ขั้นตอน)*
```yaml
---
- name: Configure web servers
  hosts: webservers
  become: yes

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start and enable Nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

---

## 3. ขั้นตอนการรัน Playbook
เปิด Terminal ในเครื่อง `ansible-control` (ไปที่โฟลเดอร์ `/home/vboxuser/ansible-lab`):
```bash
ansible-playbook -i inventory.ini install-nginx.yml --ask-become-pass
```
เมื่อขึ้น `BECOME password:` ให้ใส่รหัสผ่าน sudo (เช่น `vboxuser` หรือรหัสของเครื่องที่คุณตั้งไว้)

---

## 4. ขั้นตอนการเปลี่ยนข้อความหน้าเว็บเป็นชื่อของคุณ
ในภาพของอาจารย์ ข้อความหน้าเว็บจะขึ้นเป็น:
`674259006 Nattaphon Klinjun`

สามารถทำได้ 2 วิธี:

### วิธีที่ 1: รันคำสั่งเปลี่ยนข้อความที่เครื่อง web01 โดยตรง
รันคำสั่งนี้บนเครื่อง `web01` (หรือ ssh เข้าไปแก้):
```bash
sudo sed -i 's/Welcome to nginx!/674259006 Nattaphon Klinjun/g' /var/www/html/index.nginx-debian.html
```
หรือเปิดแก้ด้วย nano:
```bash
sudo nano /var/www/html/index.nginx-debian.html
```
แล้วเปลี่ยนบรรทัด `<h1>Welcome to nginx!</h1>` เป็น:
`<h1>674259006 Nattaphon Klinjun</h1>` จากนั้นกด `Ctrl+O` บันทึก และ `Ctrl+X` ออก
