---
layout: post
title: "Solutions of Shady Puzzles HD! app"
date: 2012-04-14 09:45
category: "!正业"
tags: [R]
---
{% include JB/setup %}

为了多凑几篇博文，同时也不忍让某些代码在硬盘中发霉，准备把已经躺了四个月的计算[Shady Puzzle HD!](http://itunes.apple.com/us/app/shady-puzzles-hd!/id387924572?mt=8 "Shady Puzzle")结果的代码发上来。方法比较原始，很烦很暴力的穷举法，稍微减少了判断的次数，没开并行。

效果是这样的：
<img src="/assets/images/SP_HD.png" alt="效果" title="效果" width="60%" />

代码

	decompos<- function(number,groups) {
	  res<- matrix(nrow=0,ncol=groups)
	  if(groups<1) stop("Parameter 'groups' MUST BE >=1")
	  if(groups==1) {
	    res<- rbind(res,number)
	  }else{
	    for(i in 0:number) {
	      tmp<- c(i,number-i)
	      if(length(tmp)<groups) {
	        tmp2<- decompos(number-i,groups-1)
	        tmp<- cbind(matrix(rep(i,nrow(tmp2)),ncol=1),tmp2)
	      }
	      res<- rbind(res,unname(tmp))
	    }
	  }
	  colnames(res)<- paste('Group.',1:ncol(res),sep='')
	  rownames(res)<- paste('Method.',1:nrow(res),sep='')
	  return(res)
	}
	
	no.cond.init<- function(yes.cond) {
	  row.num<- length(yes.cond$row)
	  col.num<- length(yes.cond$col)
	  no.cond<- list(row=rep(list(NULL),row.num),col=rep(list(NULL),col.num))
	  cond.len<- lapply(yes.cond,function(x) unlist(lapply(x,function(y) ifelse(identical(y,0),1,1+length(y)))))
	  for(i in 1:row.num) no.cond$row[i]<- list(rep(1,cond.len$row[i]))
	  for(i in 1:col.num) no.cond$col[i]<- list(rep(1,cond.len$col[i]))
	  no.cond$row<- lapply(no.cond$row,
	             function(x) {
	              if(length(x)>1) {
	                x[1]<- x[length(x)]<- 0
	              }else{
	                x<- col.num
	              }
	              return(x)
	            })
	  no.cond$col<- lapply(no.cond$col,
	             function(x) {
	              if(length(x)>1) {
	                x[1]<- x[length(x)]<- 0
	              }else{
	                x<- row.num
	              }
	              return(x)
	            })
	  return(no.cond)
	}
	
	get.freedom<- function(curr.yes,curr.no) {
	  row.num<- length(curr.yes$row)
	  col.num<- length(curr.yes$col)
	  degree<- list(row=rep(NA,row.num),col=rep(NA,col.num))
	  for(i in 1:row.num) degree$row[i]<- col.num-sum(curr.yes$row[[i]],curr.no$row[[i]])
	  for(i in 1:col.num) degree$col[i]<- row.num-sum(curr.yes$col[[i]],curr.no$col[[i]])
	  return(degree)
	}
	
	gen.candidate<- function(curr.no,curr.freedom) {
	  row.num<- length(curr.no$row)
	  col.num<- length(curr.no$col)
	  candidate<- list(row=rep(list(NULL),row.num),col=rep(list(NULL),col.num))
	  for(i in 1:row.num) candidate$row[i]<- list(decompos(curr.freedom$row[i],length(curr.no$row[[i]])))
	  for(i in 1:col.num) candidate$col[i]<- list(decompos(curr.freedom$col[i],length(curr.no$col[[i]])))
	  return(candidate)
	}
	
	fill.board<- function(yes.cond,no.cond,candi) {
	  row.num<- length(candi$row)
	  col.num<- length(candi$col)
	  candi.num<- lapply(candi,function(x) unlist(lapply(x,function(y) nrow(y))))
	  total.num<- unlist(lapply(candi.num,prod))
	  fill.direction<- which.min(total.num)
	  fill.candi<- candi[[fill.direction]]
	  fill.order<- order(candi.num[[fill.direction]])
	  solution<- rep(list(NULL),length(candi[[fill.direction]]))
	  res<- fill.recursive(solution,fill.order,fill.candi,yes.cond[[fill.direction]],no.cond[[fill.direction]],yes.cond[[-fill.direction]])
	  cat('\n')
	  if(res$solved==T) {
	    cat("*** Problem solved! ***\n")
	    dim.names<- lapply(yes.cond,function(x) unlist(lapply(x,function(y) paste(y,collapse=','))))
	    if(fill.direction==1) {
	      solution<- do.call(rbind,res$solution)
	      dimnames(solution)<- dim.names
	      return(solution)
	    }else{
	      solution<- do.call(cbind,res$solution)
	      dimnames(solution)<- dim.names
	      return(solution)
	    }
	  }else{
	    cat("*** No solutions ***\n")
	  }
	  return(NULL)
	}
	
	fill.recursive<- function(curr.sol,fill.order,curr.candi,yes.list,no.list,check.cond) {
	  num<- length(curr.sol)
	  num.to.fill<- sum(unlist(lapply(curr.sol,is.null)))
	  if(num.to.fill==0) {
	  # All filled, check it!
	    if(exists('tried')==T) {
	      tried<<- tried+1
	      if(all.shots>20) {
	        if(tried%%floor(all.shots/20)==0) cat("=> ",tried/floor(all.shots/20)*5,'% ',sep='')
	      }
	    }
	    if(check.board(curr.sol,check.cond)==TRUE) {
	    # Find solution
	      return(list(solved=T,solution=curr.sol,candidate=curr.candi))
	    }else{
	      return(list(solved=F,solution=NULL,candidate=NULL))
	    }
	  }else{
	    curr.id<- fill.order[num-num.to.fill+1]
	    for(i in 1:nrow(curr.candi[[curr.id]])) {
	      curr.sol[[curr.id]]<- parse.solution(yes.list[[curr.id]],no.list[[curr.id]]+curr.candi[[curr.id]][i,])
	      res<- fill.recursive(curr.sol,fill.order,curr.candi,yes.list,no.list,check.cond)
	      if(res$solved==T) return(list(solved=T,solution=res$solution,candidate=res$candidate))
	    }
	    return(list(solved=F,solution=NULL,candidate=NULL))
	  }
	  stop("No return!")
	}
	
	check.board<- function(solution,check.cond) {
	  sol.mat<- do.call(cbind,solution)
	  flag<- T
	  for(i in 1:length(check.cond)) {
	    tmp<- unlist(strsplit(paste(sol.mat[i,],collapse=''),'0'))
	    tmp<- tmp[tmp!='']
	    sol.str<- paste(nchar(tmp),collapse=',')
	    sol.str<- ifelse(sol.str=='','0',sol.str)
	    chk.str<- paste(check.cond[[i]],collapse=',')
	    if(sol.str!=chk.str) {
	      flag<- F
	      break
	    }
	  }
	#    cat('\n\n')
	  return(flag)
	}
	
	parse.solution<- function(yes.vec,no.vec) {
	  len<- length(yes.vec)+length(no.vec)
	  sol<- rep(NA,sum(yes.vec,no.vec))
	  for(i in 1:len) {
	    if(i%%2==1) {
	      if(no.vec[ceiling(i/2)]!=0) sol[seq(from=which(is.na(sol))[1],by=1,length=no.vec[ceiling(i/2)])]<- 0
	    }else{
	      sol[seq(from=which(is.na(sol))[1],by=1,length=yes.vec[ceiling(i/2)])]<- 1
	    }
	  }
	  return(sol)
	}
	
	solve.game<- function(conditions) {
	  yes.cond<- conditions
	  no.cond<- no.cond.init(yes.cond)
	  freedom<- get.freedom(yes.cond,no.cond)
	  candidate<- gen.candidate(no.cond,freedom)
	  print(sprintf("At most %d possibilities",all.shots<<- min(unlist(lapply(lapply(candidate,function(x) unlist(lapply(x,function(y) nrow(y)))),prod)))))
	  tried<<- 0
	  cat('0% ',sep='')
	  print(system.time(res<- fill.board(yes.cond,no.cond,candidate)))
	  print(sprintf("Tried: %d",tried))
	  print(res)
	  plot.game(res)
	  return(res)
	}
	
	plot.game<- function(solution) {
	  row.num<- nrow(solution)
	  col.num<- ncol(solution)
	  plot(0,0,type='n',xlim=c(-1,col.num),ylim=c(0,row.num+1),xlab='',ylab='',asp=1)
	  abline(v=0:col.num,h=0:row.num)
	  text(-0.5,(1:row.num)-0.8,rownames(solution))
	  text((1:col.num)-0.8,row.num+0.5,colnames(solution))
	  for(i in 1:row.num) {
	    for(j in 1:col.num) {
	      symbol<- ifelse(solution[i,j]==1,'O','X')
	      color<- ifelse(solution[i,j]==1,'blue','red')
	      text(-0.5+j,0.5+(row.num-i),symbol,cex=5,col=color)
	    }
	  }
	}
	
	solution<- solve.game(list(row=list(1,c(3,1),c(3,1),3,3),col=list(c(2,1),4,4,1,3)))

