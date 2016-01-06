---
layout: post
title: "Tightening controller test"
date: 2015-08-19 21:41:25 +0700
comments: true
categories: [ruby, tdd, rspec, rails]
---
## Problem
One day i saw a snippet of code inside controller test, which is green and sadly is a little hard to understand:

```ruby
describe BooksController, type: :controller do
  describe "GET #index" do
    before do
      expect(Book).to receive_message_chain(:published, :latest, :limit).with(10)
      create_list :book, 5, :published
      create_list :book, 3
    end

    it { get :index }
  end
end
```
After a while, i realized this tests are trying to assert query invoked in an **index** action. Unfortunately at first sight it doesn't have a clear intent about what we are going to test. 
<!--more-->
Besides that this test is not integrated with the views that will be rendered. The test still pass against this code below, if we forgot to supply view variable:

```ruby
def index
  books = Book.published.latest.limit(10) # view need @books
end
```
Ok, we have a weak controller test.
## Solution
I think there are two solutions, first is refactor the controller test to be more clear intent and tight, second is tell a controller example to render views. 

* We can apply arrange-act-assert (AAA) pattern for this test and add more assertion. AAA is simple concept: setup context -> execute thing -> verify expected behavior.
* Tell controller example to render view using **render_views** method and add more assertion with view.

```ruby
describe BooksController, type: :controller do
  describe "GET #index", :focus do
    render_views

    # arrange
    let!(:published_book){ create :book, :published }
    let!(:unpublished_book){ create :book }

    # act
    before do
      get :index
    end

    # assertion
    it "response ok & render correct view" do
      expect(response).to have_http_status :ok
      expect(response).to render_template :index
    end

    it "query latest published books" do
      expect(assigns(:books)).to include published_book
    end

    it "contain only published book title" do
      expect(response.body).to include published_book.title
      expect(response.body).not_to include unpublished_book.title
    end
  end
end
```
