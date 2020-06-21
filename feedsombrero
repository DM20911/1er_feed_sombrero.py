#!/usr/bin/env python
# -*- coding: utf-8 -*-
 
import feedparser
import ssl
import tweepy
import time
import socket, subprocess
import telebot
from telebot import types
import json

encendido=1
while encendido==1:
	consumer_key = ''
	consumer_secret = ''
	access_token = ''
	access_token_secret = ''
	TOKEN = ''
	tb = telebot.TeleBot(TOKEN)

	auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
	auth.set_access_token(access_token, access_token_secret)
	api = tweepy.API(auth)

	def certifica(title):
		bd = open('BD.txt','r')
		contenido=bd.readlines()
		bd.close()
		titleP=title+'\n'
		print (titleP)
		if titleP in contenido:
			print ('Existe tweet')
			return 'true'
		else:
			print ('NO existe tweet')
			return 'false'

	def certificaTelegram(title):
		bdt = open('BDTelegram.txt','r')
		contenido=bdt.readlines()
		bdt.close()
		titleP=title+'\n'
		print (titleP)
		if titleP in contenido:
			print ('Existe mensaje enviado')
			return 'true'
		else:
			print ('NO existe mensaje enviado')
			return 'false'

	def escribeBD(title):
		bd = open('BD.txt','a')
		bd.write('\n')
		bd.write(title)
		bd.close()

	def escribeTelegram(title):
		bdt = open('BDTelegram.txt','a')
		bdt.write('\n')
		bdt.write(title)
		bdt.close()
	    
	sitios = open('sitios.txt','r')

	for paginas in sitios:
		url = paginas
		try:
			rss = feedparser.parse(url)
			title=(rss.entries[0]['title'])
			link=(rss.entries[0]['link'])
			time.sleep(60)
			tweet = title+' '+link
			
			if certifica(title)=='false':
				try:
					api.update_status(tweet)
					escribeBD(title)

					if certificaTelegram(title)=='false':
						tb.send_message('-',tweet)
						escribeTelegram(title)
						time.sleep(20)
					else:
						continue

				except tweepy.TweepError as e:
					#code 187 tweet duplicado
					print (e.args[0][0]['code'])
					try:
						if certificaTelegram(title)=='false':
							tb.send_message('-',tweet)
							escribeTelegram(title)
							continue
						else:
							continue
					except:
						continue
			else:	
				continue
		except:
			continue

	print ('Esperando nuevo ciclo de Feeds')
	time.sleep(60*60)
			
