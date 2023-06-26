# 123
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import warnings
warnings.filterwarnings('ignore')

#读取数据
user_action = pd.read_csv('user_action.csv')
user_action.head(10)
#user_id:用户标识
#item_id:商品标识
#behavior_tupe:用户对商品的行为类型
#item_category:商品分类标识
#time:行为时间
#基本数据信息
print('数据总数:', user_action.shape[0])
print('**************')
print(user_action.info())
print('**************')
print('数据缺失情况:\n', user_action.isnull().sum())
print('**************')
print('每列类别数:\n',user_action.nunique())
print('**************')
print('开始时间:',user_action['time'].min())
print('最终时间:',user_action['time'].max())
user_action['time'] = pd.to_datetime(user_action['time'], format='%Y-%m-%d %H')
user_action['date'] = user_action.time.dt.date
user_action['month'] = user_action.time.dt.month
user_action['day'] = user_action.time.dt.day
user_action['hour'] = user_action.time.dt.hour
user_action.head(10)
#流量分析(PV,UV)
#PV：页面访问量
#UV: 独立访客

#时间尺度：日度
#计算日度PV
pv_daliy = user_action.groupby(['date'])['user_id'].agg(['count']).reset_index()
pv_daliy.columns=['date','pv_daliy']
#pv_daliy.set_index('date',inplace=True)
pv_daliy.head(10)
#计算日度UV
uv_daliy = user_action.groupby(['date'])['user_id'].nunique().reset_index()
uv_daliy.columns = ['date','uv_daliy']
#uv_daliy.set_index('date',inplace=True)
uv_daliy.head(10)
plt.rcParams['font.sans-serif']=['SimHei']#中文 
plt.rcParams['axes.unicode_minus']=False  #显示负号
fig,ax1 = plt.subplots(1,1,figsize=(8,4))
ax1.plot(pv_daliy['date'],pv_daliy['pv_daliy'],color='darkblue',label='pv_daliy')
ax1.set_ylabel('PV')
ax1.set_xlabel('Date')
ax2 = ax1.twinx()
ax2.plot(uv_daliy['date'],uv_daliy['uv_daliy'],color='orange',label='uv_daliy')
ax2.set_ylabel('UV')
ax2.legend()
ax1.legend(loc=2)
plt.title('日度PV和日度UV走势')
fig.autofmt_xdate(rotation=30)
plt.show()
pv_hour = user_action.groupby(['hour'])['user_id'].agg(['count']).reset_index()
pv_hour.columns=['hour','pv_hour']
pv_hour.head(10)
uv_hour = user_action.groupby(['hour'])['user_id'].nunique().reset_index()
uv_hour.columns = ['hour','uv_hour']
uv_hour.head(10)fig,ax1 = plt.subplots(1,1,figsize=(8,4))
ax1.plot(pv_hour['hour'],pv_hour['pv_hour'],color='darkblue',label='pv_hour')
ax1.set_ylabel('PV_hour')
ax1.set_xlabel('Hour')
ax2 = ax1.twinx()
ax2.plot(uv_hour['hour'],uv_hour['uv_hour'],color='orange',label='uv_hour')
ax2.set_ylabel('UV')
ax2.legend()
ax1.legend(loc=4)
plt.title('日度PV和日度UV走势')
fig.autofmt_xdate(rotation=30)
plt.show()
pv_day = user_action.groupby(['day'])['user_id'].agg(['count']).reset_index()
pv_day.columns=['day','pv_day']
pv_day.head(10)
uv_day = user_action.groupby(['day'])['user_id'].nunique().reset_index()
uv_day.columns = ['day','uv_day']
uv_day.head(10)
fig,ax1 = plt.subplots(1,1,figsize=(8,4))
ax1.plot(pv_day['day'],pv_day['pv_day'],color='darkblue',label='pv_day')
ax1.set_ylabel('PV_day')
ax1.set_xlabel('Day')
ax2 = ax1.twinx()
ax2.plot(uv_day['day'],uv_day['uv_day'],color='orange',label='uv_day')
ax2.set_ylabel('UV_day')
ax2.legend()
ax1.legend(loc=4)
plt.title('日度PV和日度UV走势')
fig.autofmt_xdate(rotation=30)
plt.show()
