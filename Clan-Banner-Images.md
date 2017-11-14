The Bungie Clan Banner images are generated from multiple images.  The real way to get these images can be found here https://github.com/xlxCLUxlx/Destiny2API/wiki/Building-Your-Clan-Banner:--How-To

You can also use Selenium, an HTML parser, and a base64 decoder to "cheat" at getting the clan banner images from Bungie.net.  You need Selenium because the images are generated via Javascript so a simple curl/wget won't find them.  Here is a quick and dirty Python script to show the idea.

	#!/usr/bin/env python
	"""
	Get clan banner image from bungie.net

	Download Chrome driver from
	  https://chromedriver.storage.googleapis.com/2.33/chromedriver_linux64.zip
	and unzip into working directory

	"""

	import base64
	from bs4 import BeautifulSoup
	from selenium import webdriver
	from selenium.webdriver.chrome.options import Options

	def main():
		"""
		Use Selenium and BeautifulSoup for fun and profit
		"""

		options = Options()
		options.add_argument('--headless')
		options.add_argument('start-maximized')
		driver = webdriver.Chrome(executable_path='./chromedriver', chrome_options=options)
		driver.implicitly_wait(10)
		driver.get('https://www.bungie.net/en/ClanV2?groupId=2080464')
		html = driver.execute_script('return document.body.innerHTML')
		driver.close()
		soup = BeautifulSoup(html, 'html.parser')
		bdiv = soup.find('div', {'id': 'ClanBanner'})
		img = bdiv['style']
		img = img.replace('background-image: url("data:image/png;base64,', '')
		img = img.replace('");', '')
		with open('clanbanner.png', 'wb') as outfile:
			outfile.write(base64.urlsafe_b64decode(img))

	if __name__ == '__main__':
		main()
