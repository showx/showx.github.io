---
layout: post
title: phpexcel设置样式的常用方法
date: 2017-07-21 08:06
author: admin
comments: true
categories: [Uncategorized]
---
this->exc = new PHPExcel();
$this->exc->setActiveSheetIndex(0);
$this->as = $this->exc->getActiveSheet();

以下方法基于$this->as

getStyle还有默认的getDefaultStyle
设置计算函数：
$objActSheet->setCellValue('A4', '=SUM(' . $a . ':' . $b .')');

合并单元格
mergeCells('A18:E22');
设置宽度 第几列
getColumnDimension('A')->setWidth(20);
设置单元格高度 第几行
getRowDimension(2)->setRowHeight(40);
加粗
getStyle('B1')->getFont()->setBold(true);
水平对齐
getStyle('D11')->getAlignment()->setHorizontal(PHPExcel_Style_Alignment::HORIZONTAL_RIGHT);    //HORIZONTAL_CENTER
getStyle('A18')->getAlignment()->setVertical(PHPExcel_Style_Alignment::VERTICAL_CENTER);
设置字号
getStyle()->getFont()->setSize(10);
设置边框
getStyle('A1:I20')->getBorders()->getAllBorders()->setBorderStyle(\PHPExcel_Style_Border::BORDER_THIN); 
设置边框颜色：
getStyle('D13')->getBorders()->getLeft()->getColor()->setARGB('FF993300');     getRight  getTop getBottom
单元格背景色
getStyle('A1')->getFill()->setFillType(\PHPExcel_Style_Fill::FILL_SOLID);
getStyle('A1')->getFill()->getStartColor()->setARGB('FFCAE8EA');
字体颜色相关
//斜体
getStyle->getFont()->setItalic( true);  
//字体颜色 
getStyle->getFont()->setColor( new PHPExcel_Style_Color( PHPExcel_Style_Color::COLOR_DARKGREEN ) );
getStyle( 'B1')->getFont()->getColor()->setARGB(PHPExcel_Style_Color::COLOR_WHITE);

