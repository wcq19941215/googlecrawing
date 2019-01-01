from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import os
import time
from scrapy import Selector
import urllib.request
import requests

content = input("请输入所要爬取内容:")
url = 'https://www.google.com.hk/?gws_rd=cr'
browser = webdriver.Chrome()
browser.get(url)
button = browser.find_element_by_link_text('图片').click()
input = browser.find_element_by_name('q')
input.send_keys(content)
input.send_keys(Keys.ENTER)


class GoogleCrawl():
    def __init__(self):
        self.img_path = r'C:\ImgDownLoad'  # 下载到的本地目录,根据自己所需设置
        if not os.path.exists(self.img_path):  # 路径不存在时创建一个
            os.makedirs(self.img_path)


    def main(self):
        img_source = browser.page_source
        img_source = Selector(text=img_source)
        self.parse_img(img_source)
        self.keep_down()


    def keep_down(self):
        for i in range(7, 200):  # 自己按需要设置
            pos = i * 500  # 每次向下滑动500(自己按需要设置）
            js = "document.documentElement.scrollTop=%s" % pos
            browser.execute_script(js)
            time.sleep(3)
            img_source = Selector(text=browser.page_source)
            self.parse_img(img_source)


    def parse_img(self, img_source):
        img_url_list = img_source.xpath('//div[@class="THL2l"]/../img/@src').extract()
        for each_url in img_url_list:
            if 'https' not in each_url:
                each_img_source = urllib.request.urlretrieve(each_url, '%s/%s.jpg' % (self.img_path, time.time()))  # 保存图片
            else:  # 在这里添加一个如果图片的链接不是base64的
                requests.packages.urllib3.disable_warnings()  # 去处警告
                response = requests.get(each_url, verify=False)
                with open('C:\ImgDownLoad\%s.jpg' % time.time(), 'wb') as f:
                    f.write(response.content)


if __name__ == "__main__":
    get_img = GoogleCrawl()
    get_img.main()
