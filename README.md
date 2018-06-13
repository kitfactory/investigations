# investigations
探求
### 1.目的
* xxxxの対応を決める。
* xxxxの対応を決める。

### 2.結論
* xxxxをxxxとする。
* xxxxをxxxとする。

### 3.A.I.
* xxxxxする。(担当: xxx 期日： x/xまで)
* xxxxxする。(担当: xxx 期日： x/xまで)
* xxxxxする。(担当: xxx 期日： x/xまで)

### 4.議論
* xxxxはxxxではないか？(Aさん)
→ xxxにする。

```python
import pandas as pd
import tensorflow as tf

(x_train, _),(x_test, _) = tf.keras.datasets.mnist.load_data()

print(x_train.shape)
print(x_test.shape)

x_train = x_train.reshape(-1,28,28,1)
x_test = x_test.reshape(-1,28,28,1)
x_train = x_train / 255
x_test = x_test / 255

model = tf.keras.Sequential()
model.add(tf.keras.layers.Conv2D(filters=16,kernel_size=(3,3),padding='same',input_shape=(28,28,1),activation='relu'))
model.add(tf.keras.layers.Conv2D(filters=16,kernel_size=(3,3),padding='same',activation='relu'))
model.add(tf.keras.layers.BatchNormalization())

df = pd.read_csv('test.csv')
print(df)

# val_list = [1,3]

print(type(df.values.tolist()))
print(type(df.aaa))

aaa_const = tf.constant(df.aaa.values.tolist(),name='aaa')
file_const = tf.convert_to_tensor(df.file.values.tolist(),dtype=tf.string,name='file')

dataset: tf.data.Dataset = tf.data.Dataset.from_tensor_slices((aaa_const,file_const))
iterator: tf.data.Iterator = dataset.make_one_shot_iterator()
next_element: tf.Tensor = iterator.get_next()

with tf.Session() as sess:
    try:
        print(sess.run(next_element))
        print(sess.run(next_element))
        print(sess.run(next_element))
    except tf.errors.OutOfRangeError as error:
        print('End')
```
