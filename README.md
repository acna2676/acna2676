### Hi there 👋

<!--
**acna2676/acna2676** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

```python 
import tkinter
from tkinter import messagebox  # なぜかこのインポート方法でないとmessageboxが使えない
from boto3.session import Session

session = Session(aws_access_key_id='',
                  aws_secret_access_key='',
                  region_name='ap-northeast-1')

s3 = session.resource('s3')
bucket = s3.Bucket('notification-image-store')

dynamodb = session.resource('dynamodb')
table = dynamodb.Table('notification')


// def create_item_btn_click(notification_id, language):
//     def inner():
//         print(notification_id, language)
//         response = table.put_item(
//             Item={
//                 'notification-id': notification_id,
//                 'language': language,
//             }
//         )
//     return inner

device_list_for_18a = ["TS6230", "TS8230"]


def validate_target_device(device_list):
    target_device = device_list.split(',')
    for device in target_device:
        if device not in device_list_for_18a:
            res = tkinter.messagebox.showinfo('デバイス名が不正です。')
            return False
    return target_device


def create_item_btn_click():
    # print(Entry1.get(), Entry2.get())
    notification_id = Entry1.get()
    language = Entry2.get()
    target_device = validate_target_device(Entry3.get())

    if not(notification_id and language and target_device):
        return

    response = table.put_item(
        Item={
            'notification-id': notification_id,
            'language': language,
            'target_device': target_device,
        }
    )
 
def upload_image_btn_click():
    bucket.upload_file('sample.png', 'sample.png')
    # print("uploaded !")


root = tkinter.Tk()
root.title("Notification Uploader")
root.geometry("300x200")

label = tkinter.Label(root, text="IDを入力してください")
Entry1 = tkinter.Entry(width=50)
# Entry1.insert(tkinter.END, 'notification-id')
Entry1.pack()

label = tkinter.Label(root, text="言語を入力してください")
Entry2 = tkinter.Entry(width=50)
# Entry2.insert(tkinter.END, 'language')
Entry2.pack()

label = tkinter.Label(root, text="対象機種を入力してください")
Entry3 = tkinter.Entry(width=50)
# Entry2.insert(tkinter.END, 'language')
Entry3.pack()

# btn_put_db = tkinter.Button(root, text='DBアップロード', command=create_item_btn_click(Entry1.get(), Entry2.get()))
btn_put_db = tkinter.Button(root, text='DBアップロード', command=create_item_btn_click)
btn_put_s3 = tkinter.Button(root, text='S3アップロード', command=upload_image_btn_click)
btn_put_db.pack()
btn_put_s3.pack()

root.mainloop()
```


