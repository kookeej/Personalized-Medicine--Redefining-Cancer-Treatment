๐ Personalized Medicine: Redefining Cancer Treatment
===
### *Kaggleโ Predict the effect of Genetic Variants to enable Personalized Medicine!๐*     
![image](https://user-images.githubusercontent.com/74829786/177875738-e780ded5-07b7-4b56-b2b5-d1d999bbd03f.png)
Kaggle research๋ก ๋์จ ํ๋ก์ ํธ๋ก, **๊ฐ์ธ ๋ง์ถคํ ์์ฝํ**์ ์ด์ฉํ๊ธฐ ์ํด ์งํ๋์์ต๋๋ค.    
๋จผ์  ์ผ๊ธฐ ์์ด์ด ๋ถ์๋๋ฉด, ์ ์ข์์ ์์ฒ ๊ฐ์ ์ ์ ์  ๋์ฐ๋ณ์ด๋ฅผ ๊ฐ์ง ์ ์์ต๋๋ค. ๊ทธ๋ฌ๋ ๋ฌธ์ ๋ ์ข์ ์ฑ์ฅ์ ๊ธฐ์ฌํ๋ ๋์ฐ๋ณ์ด์ ์ค์ฑ ๋์ฐ๋ณ์ด๋ฅผ ๊ตฌ๋ณํ๋ ๊ฒ์๋๋ค.    
**ํ์ฌ๋ก์จ๋ ์ด ์ ์ ์ ๋์ฐ๋ณ์ด์ ๋ํ ํด์์ ์๋์ผ๋ก ์ํ๋๊ณ  ์์ต๋๋ค. ์์ ๋ณ๋ฆฌํ์๋ ํ์คํธ ๊ธฐ๋ฐ ์์ ๋ฌธํ ๊ทผ๊ฑฐ๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ๋ชจ๋  ๋จ์ผ ์ ์ ์ ๋์ฐ๋ณ์ด๋ฅผ ์๋์ผ๋ก ๊ฒํ ํ๊ณ  ๋ถ๋ฅํด์ผํ๊ณ , ์ด๋ ๋งค์ฐ ์๋ชจ์ ์ธ ์์์๋๋ค.**    

**๋ฐ๋ผ์ MSKCC๋ ์ ์ ์  ๋ณ์ด๋ฅผ ์๋์ผ๋ก ๋ถ๋ฅํ๋ ๋จธ์ ๋ฌ๋ ์๊ณ ๋ฆฌ์ฆ์ ๊ฐ๋ฐํ๊ธฐ ์ํด Kaggle๊ณผ ํ๋ ฅํ์ฌ ์ด Competition์ ์ํํ์์ต๋๋ค.**


                                                                                                                   
***

## 1. Dataset
* ๋ฐ์ดํฐ์์ ์ ์ฒด ํฌ๊ธฐ๋ ์ด 3316์ผ๋ก ํ์ต ๋ฐ์ดํฐ์์ผ๋ก๋ ์ ์ ์์๋๋ค.
* ํ๋์ ๋ฌธ์๋น ์ต๋ ์ญ๋ง ๊ธ์๊น์ง ๋ค์ด์๊ธฐ ๋๋ฌธ์ ์ต๋ ํ ํฐ์๊ฐ 512์ธ BERT๋ฅผ ์ฌ์ฉํ๊ธฐ์๋ ๋ฌด๋ฆฌ๊ฐ ์์ต๋๋ค.
* ๋ฐ๋ผ์ **ํ ํ์คํธ๋ฅผ 2000๊ธ์(๋๋ต 512ํ ํฐ ์ด๋ด)๋ก ์ชผ๊ฐ ์๋กญ๊ฒ ๋ฌธ์๋ฅผ ๋ง๋ค์ด** Data augmentation๊ณผ BERT max token length ๋ฌธ์ ๋ฅผ ๋์์ ํด๊ฒฐํ์ต๋๋ค. ๊ทธ ๊ฒฐ๊ณผ ๋ฐ์ดํฐ์ ์๋ 3316์์ 107352๋ก ์ฆ๊ฐํ์์ต๋๋ค.
```python
# ๊ฐ ํ์คํธ๋ Gene, Variation ์ ๋ณด๋ฅผ ๊ฐ๋๋ก ํ๋ค.
# special token </s>๋ก ๊ตฌ๋ถ์ง์ด์ค๋ค.
dataset['texts'] = " </s> " + dataset['Gene'] + " </s> " + dataset['Variation'] + ' </s> ' + dataset['TEXT']
```

```python
# ๋ฐ์ดํฐ์์ ๊ธ์์ 2000์ ๊ธฐ์ค์ผ๋ก ๋ชจ๋ ๋๋  ๋ฐ์ดํฐํ๋ ์์ ์ถ๊ฐํ๋ค.
n = 2000
new_df = pd.DataFrame(columns={'ID', 'TEXT', 'NEW_TEXT', 'LABELS', 'CLASS'})
for i in range(len(dataset)):
    result = [dataset.iloc[i]['TEXT'][k * n:(k + 1) * n] for k in range((len(dataset.iloc[i]['TEXT']) + n - 1) // n )] 
    for j in range(len(result)):
        item = {'ID': dataset.iloc[i]['ID'], 'TEXT': result[j], 
                'NEW_TEXT': "</s> " + dataset.iloc[i]['Gene'] + " </s> " + dataset.iloc[i]['Variation'] + " </s> " + result[j],
                'LABELS': dataset.iloc[i]['labels'],
                'CLASS': dataset.iloc[i]['Class']}
        new_df = new_df.append(item, ignore_index=True)
```
* Class๋ 1, 2, 3...๊ณผ ๊ฐ์ ํ์์ผ๋ก ๋ํ๋์๋๋ฐ, one-hot encoding์ ํตํด ๋ ์ด๋ธ์ ์๋ก ์ ์ํด์ค๋๋ค.
* ์ต์ข์ ์ธ Dataframe์ ๋ชจ์ต์ ์๋์ ๊ฐ์ต๋๋ค.
![image](https://user-images.githubusercontent.com/74829786/177877343-6aaaba2d-4ffc-4c88-997d-c3d02b15ca66.png)



* ์ค์ ๋ก ์คํ์ ์งํํด๋ณด๋ data augmentation์ ์งํํ ๋ฐ์ดํฐ์์ ํ์ต์ํจ ๋ชจ๋ธ์ด ๊ทธ๋ ์ง ์์ ๋ชจ๋ธ๋ณด๋ค ๋ ์ฑ๋ฅ์ด ์ข์ ๊ฒ์ ํ์ธํ  ์ ์์์ต๋๋ค.    
![image](https://user-images.githubusercontent.com/74829786/177870405-2029e627-8adc-470a-bccd-7a7d8be5223b.png)


## 2. Model
* Biomedical text๋ฅผ ์ํ ์ฌ์ ํ์ต ๋ชจ๋ธ์ธ [BioBERT-Large v1.1](https://github.com/dmis-lab/biobert)๋ฅผ ์ฌ์ฉํ์์ต๋๋ค.
* ID Embedding์ ์ ์ฉํ ๋ชจ๋ธ๊ณผ ์ ์ฉํ์ง ์์ ๋ชจ๋ธ์ ์ค๊ณํ์์ต๋๋ค.


## 3. Experiments
```
epochs: 10
learning rate: 1e-4
optimizer: AdamW
scheduler: get_linear_schedule_with_warmup
loss fucntion: BCEWithLogitsLoss, log loss
```
* **ID Embedding์ ์ ์ฉํ ๊ฒฝ์ฐ**    
![image](https://user-images.githubusercontent.com/74829786/177868532-eb173fd2-4a94-46e6-a3c7-782a6c819e89.png)

* **ID Embedding์ ์ ์ฉํ์ง ์์ ๊ฒฝ์ฐ**    
![image](https://user-images.githubusercontent.com/74829786/177868684-1ef4fbeb-771d-4435-8844-ca24e6a7ccf8.png)

* ID Embedding์ ์ ์ฉํ ๊ฒฝ์ฐ๊ฐ ๊ทธ๋ ์ง ์์ ๊ฒฝ์ฐ๋ณด๋ค ์ฑ๋ฅ์ด ๋ ๋ฐ์ด๋๋ค๋ ๊ฒ์ ํ์ธํ  ์ ์์ต๋๋ค.    
![image](https://user-images.githubusercontent.com/74829786/177868219-7c2e4a80-b301-401e-aafa-97fe1669eff7.png)

***

### ๐ก ์คํ ๋ฐฉ๋ฒ

#### 1. Data Preprocessing
```python
$ python preprocessing.py \
  --train_path =TRAIN_DATASET_PATH
  --test_size  =0.1
  --max_len    =MAX_TOKEN_LENGTH  # 256
```

#### 2. Training
```python
$ python train.py \
  --epochs =10
```
