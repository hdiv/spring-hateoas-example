spring-boot-hateoas-example
=======================

Sample application to test Spring HATEOAS and HAL-FORMS support

How to build the application
============================
Clone this repo and build it (you'll need Git and Maven installed):

    git clone git://github.com/hdiv/spring-hateoas-example.git
    cd spring-hateoas-example
    mvn package
    
Run application packaged as a jar:
    
    java -jar target/spring-boot-hateoas-example.jar

Open [http://localhost:8080/api/](http://localhost:8080/api/) in your favorite browser.


Entry point GET /api/
============================

	{
		"_links": {
		    "halforms:make-transfer": {
		      "href": "http://127.0.0.1:8080/api/transfer"
		    },
		    "halforms:list-transfers": {
		      "href": "http://127.0.0.1:8080/api/transfer"
		    },
		    "halforms:list-after-date-transfers": {
		      "href": "http://127.0.0.1:8080/api/transfer/filter{?dateFrom,dateTo,status}",
		      "templated": true
		    },
		    "curies": [
		      {
		        "href": "http://127.0.0.1:8080/doc/{rel}",
		        "name": "halforms",
		        "templated": true
		      }
		    ]
		}
	}

Open this URL the application will show the main operations that can be done:

* [make-transfer](http://localhost:8080/api/transfer): URL to POST a new transfer
* [list-transfers](http://127.0.0.1:8080/api/transfer): URL to list all transfers (using GET)
* [list-after-date-transfers](http://127.0.0.1:8080/api/transfer/filter{?dateFrom,dateTo,status}): URL to list transfers matching a filter (using GET)

Additionally documentation links are provided in this URL template http://127.0.0.1:8080/doc/{rel}, for now only HAL-FORMS documents are properly documented, in this first level the following documentation forms are available


	{
	  "_embedded": {
	    "halforms:cashAccountList": [
	      {
	        "number": "1111201202332",
	        "availableBalance": 1250.3,
	        "description": "Cash Account 1"
	      },
	      {
	        "number": "2222230102332",
	        "availableBalance": 250.3,
	        "description": "Cash Account 2"
	      },
	      {
	        "number": "3333299999332",
	        "availableBalance": 7250.3,
	        "description": "Cash Account 3"
	      },
	      {
	        "number": "5555501202332",
	        "availableBalance": 5250.9,
	        "description": "Cash Account 4"
	      }
	    ]
	  },
	  "_links": {
	    "self": {
	      "href": "http://127.0.0.1:8080/doc/make-transfer"
	    },
	    "curies": [
	      {
	        "href": "http://127.0.0.1:8080/doc/{rel}",
	        "name": "halforms",
	        "templated": true
	      }
	    ]
	  },
	  "_templates": {
	    "default": {
	      "method": "POST",
	      "properties": [
	        {
	          "name": "amount",
	          "readOnly": false,
	          "required": true
	        },
	        {
	          "name": "date",
	          "readOnly": false
	        },
	        {
	          "name": "description",
	          "readOnly": false,
	          "required": true
	        },
	        {
	          "name": "email",
	          "readOnly": false
	        },
	        {
	          "name": "fromAccount",
	          "readOnly": false,
	          "suggest": {
	            "embedded": "halforms:cashAccountList",
	            "prompt-field": "description",
	            "value-field": "number"
	          }
	        },
	        {
	          "name": "id",
	          "readOnly": true
	        },
	        {
	          "name": "status",
	          "readOnly": false,
	          "suggest": [
	            {
	              "value": "COMPLETED",
	              "prompt": "COMPLETED"
	            },
	            {
	              "value": "REFUSED",
	              "prompt": "REFUSED"
	            },
	            {
	              "value": "PENDING",
	              "prompt": "PENDING"
	            }
	          ]
	        },
	        {
	          "name": "toAccount",
	          "readOnly": false,
	          "suggest": {
	            "href": "http://127.0.0.1:8080/api/cashaccounts/filter{?filter}",
	            "prompt-field": "description"
	          }
	        },
	        {
	          "name": "type",
	          "readOnly": false,
	          "suggest": [
	            {
	              "value": "NATIONAL",
	              "prompt": "NATIONAL"
	            },
	            {
	              "value": "INTERNATIONAL",
	              "prompt": "INTERNATIONAL"
	            }
	          ]
	        }
	      ]
	    }
	  }
	}



* [halforms:make-transfer](http://127.0.0.1:8080/doc/make-transfer): HAL-FORMS document for POSTing a transfer

* [halforms:list-after-date-transfers](http://127.0.0.1:8080/doc/list-after-date-transfers): HAL-FORMS document for filtering transfers by date (GET)


	{
	  "_links": {
	    "self": {
	      "href": "http://127.0.0.1:8080/doc/list-after-date-transfers"
	    }
	  },
	  "_templates": {
	    "default": {
	      "method": "GET",
	      "properties": [
	        {
	          "name": "dateFrom",
	          "readOnly": false
	        },
	        {
	          "name": "dateTo",
	          "readOnly": false
	        },
	        {
	          "name": "status",
	          "readOnly": false,
	          "suggest": [
	            {
	              "value": "COMPLETED",
	              "prompt": "COMPLETED"
	            },
	            {
	              "value": "REFUSED",
	              "prompt": "REFUSED"
	            },
	            {
	              "value": "PENDING",
	              "prompt": "PENDING"
	            }
	          ]
	        }
	      ]
	    }
	  }
	}

	
Listing transfers GET /api/transfer
===================================

Next step for browsing the API is going into api/transfer to list the current transfers


	{
	  "_embedded": {
	    "halforms:transferList": [
	      {
	        "id": 1,
	        "fromAccount": "1111201202332",
	        "toAccount": "3333299999332",
	        "description": "Transfer1",
	        "amount": 0.0,
	        "date": 0,
	        "type": "INTERNATIONAL",
	        "status": "COMPLETED",
	        "email": "ander@dummy.com",
	        "_links": {
	          "self": {
	            "href": "http://127.0.0.1:8080/api/transfer/1"
	          },
	          "halforms:modify": {
	            "href": "http://127.0.0.1:8080/api/transfer/1"
	          },
	          "halforms:delete": {
	            "href": "http://127.0.0.1:8080/api/transfer/1"
	          }
	        }
	      },
	      {
	        "id": 2,
	        "fromAccount": "3333299999332",
	        "toAccount": "1111201202332",
	        "description": "Transfer2",
	        "amount": 0.0,
	        "date": 1000,
	        "type": "NATIONAL",
	        "status": "PENDING",
	        "email": "ander@dummy.com",
	        "_links": {
	          "self": {
	            "href": "http://127.0.0.1:8080/api/transfer/2"
	          },
	          "halforms:modify": {
	            "href": "http://127.0.0.1:8080/api/transfer/2"
	          },
	          "halforms:delete": {
	            "href": "http://127.0.0.1:8080/api/transfer/2"
	          }
	        }
	      }
	    ]
	  },
	  "_links": {
	    "self": {
	      "href": "http://127.0.0.1:8080/api/transfer"
	    },
	    "halforms:list-after-date-transfers": {
	      "href": "http://127.0.0.1:8080/api/transfer/filter{?dateFrom,dateTo,status}",
	      "templated": true
	    },
	    "curies": [
	      {
	        "href": "http://127.0.0.1:8080/doc/{rel}",
	        "name": "halforms",
	        "templated": true
	      }
	    ]
	  }
	}


In this URL the following new operations are defined

* [get-transfer](http://127.0.0.1:8080/api/transfer/{id}): URL to GET each transfer, shown as self link of each resource
* [modify](http://127.0.0.1:8080/api/transfer/{id}): URL to modify (PUT) each transfer, shown as modify link of each resource
* [delete](http://127.0.0.1:8080/api/transfer/{id}): URL to DELETE each transfer, shown as delete link of each resource

Additionally, again, documentation links are provided in this level also

* [halforms:modify](http://127.0.0.1:8080/doc/modify): HAL-FORMS document for PUTing a transfer


	{
	  "_embedded": {
	    "halforms:cashAccountList": [
	      {
	        "number": "1111201202332",
	        "availableBalance": 1250.3,
	        "description": "Cash Account 1"
	      },
	      {
	        "number": "2222230102332",
	        "availableBalance": 250.3,
	        "description": "Cash Account 2"
	      },
	      {
	        "number": "3333299999332",
	        "availableBalance": 7250.3,
	        "description": "Cash Account 3"
	      },
	      {
	        "number": "5555501202332",
	        "availableBalance": 5250.9,
	        "description": "Cash Account 4"
	      }
	    ]
	  },
	  "_links": {
	    "self": {
	      "href": "http://127.0.0.1:8080/doc/modify"
	    },
	    "curies": [
	      {
	        "href": "http://127.0.0.1:8080/doc/{rel}",
	        "name": "halforms",
	        "templated": true
	      }
	    ]
	  },
	  "_templates": {
	    "default": {
	      "method": "PUT",
	      "properties": [
	        {
	          "name": "amount",
	          "readOnly": false,
	          "required": true
	        },
	        {
	          "name": "date",
	          "readOnly": false
	        },
	        {
	          "name": "description",
	          "readOnly": false,
	          "required": true
	        },
	        {
	          "name": "email",
	          "readOnly": false
	        },
	        {
	          "name": "fromAccount",
	          "readOnly": false,
	          "suggest": {
	            "embedded": "halforms:cashAccountList",
	            "prompt-field": "description",
	            "value-field": "number"
	          }
	        },
	        {
	          "name": "id",
	          "readOnly": true
	        },
	        {
	          "name": "status",
	          "readOnly": false,
	          "suggest": [
	            {
	              "value": "COMPLETED",
	              "prompt": "COMPLETED"
	            },
	            {
	              "value": "REFUSED",
	              "prompt": "REFUSED"
	            },
	            {
	              "value": "PENDING",
	              "prompt": "PENDING"
	            }
	          ]
	        },
	        {
	          "name": "toAccount",
	          "readOnly": false,
	          "suggest": {
	            "href": "http://127.0.0.1:8080/api/cashaccounts/filter{?filter}",
	            "prompt-field": "description"
	          }
	        },
	        {
	          "name": "type",
	          "readOnly": false,
	          "suggest": [
	            {
	              "value": "NATIONAL",
	              "prompt": "NATIONAL"
	            },
	            {
	              "value": "INTERNATIONAL",
	              "prompt": "INTERNATIONAL"
	            }
	          ]
	        }
	      ]
	    }
	  }
	}


* [halforms:delete](http://127.0.0.1:8080/doc/delete): HAL-FORMS document for DELETEing a transfer


	{
	  "_links": {
	    "self": {
	      "href": "http://127.0.0.1:8080/doc/delete"
	    }
	  },
	  "_templates": {
	    "default": {
	      "method": "DELETE"
	    }
	  }
	}
	

	