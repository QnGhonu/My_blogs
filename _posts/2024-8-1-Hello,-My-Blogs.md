---
title: "你好，我的个人博客网站！"
date: 2024-8-1
---
作为第一篇文章，为了构建网站，所以随便找了一篇之前的文章：

## 基于C++的桌面窗体构建
···
#include <graphics.h>
#include <stdio.h>
#include <string>

class window {
	private:
		typedef unsigned uint;
		typedef long long lint;
		typedef unsigned long long ulint;
		typedef const int cit;
		uint control;
		uint width, height;
		uint textSize;
		uint textx, texty;
		IMAGE img;
		bool couldFlushBoard;
	public:
		window(int userset = 1) {
			control = userset;
			width = 400 * control;
			height = 300 * control;
			textSize = 20 * control;
			textx = 50 * control;
			texty = 50 * control;
			couldFlushBoard = false;
		}
		struct location {
			uint *_x, *_y;
		};
		void show() {
			initgraph(width, height);
			clearDevice();
		}
		void close() {
			closegraph();
		}
		void loadImage(char* imagePath) {
			loadimage(&img, imagePath);
			if (img.getheight() != 0) couldFlushBoard = true;
		}
	protected:
		uint getControlValue() {
			return control;
		}
		uint getWidth() {
			return width;
		}
		uint getHeight() {
			return height;
		}
		location getTextLocation() {
			location toReturn;
			toReturn._x = &textx;
			toReturn._y = &texty;
			return toReturn;
		}
		void clearDevice() {
			setbkcolor(RGB(255, 255, 255));
			settextcolor(RGB(0, 0, 255));
			settextstyle(textSize, 0, _T("微软雅黑"));
			cleardevice();
			if (couldFlushBoard) putimage(0, 0, &img);
		}
};

class printer : window {
	public:
		void putwords(std::string words) {
			for (unsigned index=0; index<words.size(); index++) {
				char& word = words[index];
				if (word == '\n') {
					*getTextLocation()._y += getControlValue() * 50;
					return;
				}
				outtextxy(*getTextLocation()._x, *getTextLocation()._y, word);
				*getTextLocation()._x += getControlValue() * 20;
			}
		}
};
//MOUSEMSG msg = GetMouseMsg();
//x = (msg.x - 50 + 15) / 30;
//y = (msg.y - 50 + 15) / 30;
int main() {
	window Qmain(2);
	printer qout;
	Qmain.show();

	qout.putwords("Hello,\nNice to meet you!\n");

	getchar();
	Qmain.close();
	return 0;
}
···
