#include "my_malloc.h"
#include <stdio.h>

void *bf_malloc(size_t size){
  if(start_f == NULL){
    new_brk(NULL);
  }
  meta_t *curr = start_f;
  while(curr != NULL){
    meta_t *tochange = NULL;
    size_t min = (size_t)-1;

    if(curr->b_size >= size + sizeof(meta_t)){
      tochange = curr;
      min = curr->b_size;
    }
    while(curr->next_f != NULL){
      if(size + sizeof(meta_t) == curr->b_size){
	tochange = curr;
	break;
      }
      if(curr->next_f->b_size >= size + sizeof(meta_t)){
	if(curr->next_f->b_size < min){
	  tochange = curr->next_f;
	  min = curr->next_f->b_size;
	}
      }
      curr = curr->next_f;
    }
    if(tochange != NULL){
      if(tochange->b_size <= size + 2*sizeof(meta_t)){
	curr = tochange;
	if(curr->prev_f != NULL) {
	  curr->prev_f->next_f = curr->next_f;
	}
	else{
	  start_f = curr->next_f;
	}
	if(curr->next_f != NULL) curr->next_f->prev_f = curr->prev_f;
	curr->next_f = NULL;
	curr->prev_f = NULL;
	curr->ava = 0;
	return curr + 1;
      }	
      curr = tochange;
      char *te = (char*)curr + size + sizeof(meta_t);
      meta_t *temp = (meta_t*)te;
      temp->prev = curr;
      temp->next = curr->next;
      if(temp->next != NULL) {
	temp->next->prev = temp;
      }
      else{
	tail = temp;
      }
      temp->prev_f = curr->prev_f;
      if(temp->prev_f != NULL) {
	temp->prev_f->next_f = temp;
      }
      else{
	start_f = temp;
      }
      temp->next_f = curr->next_f;
      if(temp->next_f != NULL) temp->next_f->prev_f = temp;
      temp-> ava = 1;
      temp->b_size = curr->b_size - size - sizeof(meta_t);
      
      curr->next = temp;
      curr->next_f = NULL;
      curr->prev_f = NULL;
      curr-> ava = 0;
      curr-> b_size = size + sizeof(meta_t);
      return curr + 1;

    }
    else{
      new_brk(curr);
    }
  }
  printf("error\n");
  return NULL;
}

void *ff_malloc(size_t size){
  if(start_f == NULL){
    new_brk(NULL);
  }
  meta_t *curr = start_f;
  while(curr != NULL){

    if(curr->b_size >= size + sizeof(meta_t)){      
      if(curr->b_size <= size + 2*sizeof(meta_t)){
	if(curr->prev_f != NULL) {
	  curr->prev_f->next_f = curr->next_f;
	}
	else{
	  start_f = curr->next_f;
	}
	if(curr->next_f != NULL) curr->next_f->prev_f = curr->prev_f;
	curr->next_f = NULL;
	curr->prev_f = NULL;
	curr->ava = 0;
	return curr + 1;
      }
      
      char *te = (char*)curr + size + sizeof(meta_t);
      meta_t *temp = (meta_t*)te;
      temp->prev = curr;
      temp->next = curr->next;
      if(temp->next != NULL) {
	temp->next->prev = temp;
      }
      else{
	tail = temp;
      }
      temp->prev_f = curr->prev_f;
      if(temp->prev_f != NULL) {
	temp->prev_f->next_f = temp;
      }
      else{
	start_f = temp;
      }
      temp->next_f = curr->next_f;
      if(temp->next_f != NULL) temp->next_f->prev_f = temp;
      temp-> ava = 1;
      temp->b_size = curr->b_size - size - sizeof(meta_t);
      
      curr->next = temp;
      curr->next_f = NULL;
      curr->prev_f = NULL;
      curr-> ava = 0;
      curr-> b_size = size + sizeof(meta_t);
      return curr + 1;
    }
    else{
      if(curr->next_f == NULL){
	new_brk(curr);
      }
      else{
	curr = curr->next_f;
      }
    }
  }
  printf("error\n");
  return NULL;
}

void new_brk(meta_t *curr){
  meta_t *temp = sbrk(512);
  temp->next = NULL;
  temp->prev_f = curr;
  temp->b_size = 512;
  temp->next_f = NULL;
  temp->ava = 1;
  int i = 0;
  if(tail != NULL){
    if(tail->ava == 1){
      tail->b_size += 512;
      i = 1;
    }
    else{
      temp->prev = tail;
      tail->next = temp;
      tail = temp;
    }
  }
  else{
    tail = temp;
    temp->prev = NULL;
    start = temp;
  }
  if(curr != NULL){
    if(i == 0){
      curr->next_f = temp;
    }
  }
  else{
    start_f = temp;
  }
}

void bf_free(void *ptr){
  meta_t *curr = ptr;
  curr--;
  curr->ava = 1;
  if(curr->prev != NULL){
    
    if(curr->prev->ava == 1){
      meta_t *prev_f = curr->prev;
      if(curr->next != NULL && curr->next->ava == 1){
	if(tail == curr->next){
	  tail = prev_f;
	}
	prev_f->next = curr->next->next;
	if(curr->next->next != NULL) curr->next->next->prev = prev_f;
	if(curr->next->next_f != NULL) curr->next->next_f->prev_f = prev_f;
	prev_f->next_f = curr->next->next_f;
	prev_f->b_size = prev_f->b_size + curr->b_size + curr->next->b_size;
      }
      else{
	if(tail == curr){
	  tail = prev_f;
	}
	prev_f->next = curr->next;
	if(curr->next != NULL) curr->next->prev = prev_f;
	prev_f->b_size += curr->b_size;
      }
      curr->next = NULL;
    }//delete
    else{
      meta_t *temp = start_f;
      if(temp == NULL){
	start_f = curr;
      }
      else{
	if(temp > curr){
	  curr->next_f = start_f;
	  start_f->prev_f = curr;
	  start_f = curr;
	}
	else{
	  while(temp < curr){
	    if(temp->next_f == NULL){
	      
	      temp->next_f = curr;
	      curr->prev_f = temp;
	      curr->next_f = NULL;
	      break;
	    }
	    else if(temp->next_f > curr){
	      curr->next_f = temp->next_f;
	      temp->next_f = curr;
	      curr->prev_f = temp;
	      break;
	    }
	    else{
	      temp = temp->next_f;
	    }
	  }
	}
      }
    }//
  }
  else{
    curr->next_f = start_f;
    start_f = curr;
  }
  if(curr->next != NULL){
    if(curr->next->ava == 1){
      if(tail == curr->next){
	tail = curr;
      }
      curr->b_size += curr->next->b_size;
      if(curr->next->next != NULL) curr->next->next->prev = curr;
      if(curr->next->next_f != NULL) curr->next->next_f->prev_f = curr;
      curr->next_f = curr->next->next_f;
      curr->next = curr->next->next;
    }
    else{
      meta_t *temp = curr->next_f;
      if(temp != NULL){
	temp->prev_f = curr;
      }
    }
  }   
}

void ff_free(void *ptr){
  bf_free(ptr);
}
    
unsigned long get_data_segment_size(){
  unsigned long size = 0;
  unsigned long size_used = 0;
  meta_t *curr = start;
  while(curr != NULL){
    if(curr->ava == 0){
      size_used = size_used + curr->b_size;
    }
    size = size + curr->b_size;
    curr = curr->next;
  }
  return size;
}

unsigned long get_data_segment_free_space_size(){
  unsigned long size = 0;
  meta_t *curr = start_f;
  while(curr != NULL){
    size = size + curr->b_size - sizeof(meta_t);
    curr = curr->next_f;
  }
  return size;
}
