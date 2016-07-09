---
author: 飞雪无情
comments: true
date: 2013-01-05 04:15:14+00:00
layout: post
slug: android-viewgroup-setdescendantfocusability
title: Android ViewGroup.setDescendantFocusability函数
wordpress_id: 42
categories:
- Android
tags:
- Android
---


这个函数是在ViewGroup里定义的，主要用于控制child View获取焦点的能力，比如是否阻止child View获取焦点。

他有三个常量可供设置



	
  1. FOCUS_BEFORE_DESCENDANTS ViewGroup本身先对焦点进行处理，如果没有处理则分发给child View进行处理

	
  2. FOCUS_AFTER_DESCENDANTS 先分发给Child View进行处理，如果所有的Child View都没有处理，则自己再处理

	
  3. FOCUS_BLOCK_DESCENDANTS ViewGroup本身进行处理，不管是否处理成功，都不会分发给ChildView进行处理




    我们看下这个方法的实现






    
    public void setDescendantFocusability(int focusability) {
            switch (focusability) {
                case FOCUS_BEFORE_DESCENDANTS:
                case FOCUS_AFTER_DESCENDANTS:
                case FOCUS_BLOCK_DESCENDANTS:
                    break;
                default:
                    throw new IllegalArgumentException("must be one of FOCUS_BEFORE_DESCENDANTS, "
                            + "FOCUS_AFTER_DESCENDANTS, FOCUS_BLOCK_DESCENDANTS");
            }
            mGroupFlags &= ~FLAG_MASK_FOCUSABILITY;
            mGroupFlags |= (focusability & FLAG_MASK_FOCUSABILITY);
        }


可以看到，只有这三个常量可以设置，不是这三个常量会抛出异常的。

设置后，会在requestFocus(int direction, Rect previouslyFocusedRect) 方法里根据设置进行相应的处理。来看下实现





    
    public boolean requestFocus(int direction, Rect previouslyFocusedRect) {
            if (DBG) {
                System.out.println(this + " ViewGroup.requestFocus direction="
                        + direction);
            }
            int descendantFocusability = getDescendantFocusability();
    
            switch (descendantFocusability) {
                case FOCUS_BLOCK_DESCENDANTS:
                    return super.requestFocus(direction, previouslyFocusedRect);
                case FOCUS_BEFORE_DESCENDANTS: {
                    final boolean took = super.requestFocus(direction, previouslyFocusedRect);
                    return took ? took : onRequestFocusInDescendants(direction, previouslyFocusedRect);
                }
                case FOCUS_AFTER_DESCENDANTS: {
                    final boolean took = onRequestFocusInDescendants(direction, previouslyFocusedRect);
                    return took ? took : super.requestFocus(direction, previouslyFocusedRect);
                }
                default:
                    throw new IllegalStateException("descendant focusability must be "
                            + "one of FOCUS_BEFORE_DESCENDANTS, FOCUS_AFTER_DESCENDANTS, FOCUS_BLOCK_DESCENDANTS "
                            + "but is " + descendantFocusability);
            }
        }


通过这里的实现可以看到上面定义的三个常量设置的意思。。
