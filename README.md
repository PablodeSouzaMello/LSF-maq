# LSF-maqfrom selenium import webdriver 
from selenium.webdriver import Keys
from selenium.webdriver.common.by import By
navegador = webdriver.Chrome()
navegador.get("https://outlook.office.com/mail/")
print('Login no email')
login = input()
navegador.find_element(By.XPATH,'//*[@id="i0116"]').send_keys(login)
navegador.find_element(By.XPATH,'//*[@id="i0116"]').send_keys(Keys.RETURN)
print('Senha do email')
senha = input()
navegador.find_element(By.XPATH,'//*[@id="i0118"]').send_keys(senha)
navegador.find_element(By.XPATH,'//*[@id="i0118"]').send_keys(Keys.RETURN)
navegador.find_element(By.XPATH,'//*[@id="idBtn_Back"]').click()
import win32com.client as win32
from pathlib import Path
import pandas as pd
import os
import codecs
output_dir = Path.cwd()/"output"
output_dir.mkdir(parents=True, exist_ok=True) 
outlook = win32.Dispatch('outlook.application')
navegador.get('https://www.microsoft365.com/launch/excel?auth=2')
navegador.find_element(By.XPATH,'//*[@id="F57D5D29-4967-495C-A648-F7AB66BF8572"]/span/div').click()
print('Lista de Arquivos')
navegador.find_element(By.XPATH,'//*[@id="my-content"]/div[2]/div[2]/div[3]/div[2]').click()
Laminador de Steel Frame 4.0
