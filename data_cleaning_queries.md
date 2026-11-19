# MongoDB Data Cleaning Queries

A reference of the queries used to clean and de-duplicate the `cleaning` collection. All queries are reproduced exactly as written in the source file.

## 1. Clean the `remarks` column

```javascript
db.cleaning.updateMany(
    {},
    [
        {
            $set: {
                remarks: {
                    $let: {
                        vars: {
                            val: {
                                $trim: {
                                    input: {
                                        $toString: {
                                            $ifNull: ["$remarks", ""]
                                        }
                                    }
                                }
                            }
                        },
                        in: {
                            $cond: [
                                {
                                    $in: [
                                        { $toLower: "$$val" },
                                        ["", "---", "???", "na", "n/a", "tbd"]
                                    ]
                                },
                                "Unknown",
                                {
                                    $switch: {
                                        branches: [
                                            { case: { $eq: ["$$val", "accessories added"] }, then: "Accessories Added" },
                                            { case: { $eq: ["$$val", "color changed"] }, then: "Color Changed" },
                                            { case: { $eq: ["$$val", "customer postponed"] }, then: "Customer Postponed" },
                                            { case: { $eq: ["$$val", "delivery next week"] }, then: "Delivery Next Week" },
                                            { case: { $eq: ["$$val", "document missing"] }, then: "Document Missing" },
                                            { case: { $eq: ["$$val", "duplicate"] }, then: "Duplicate" },
                                            { case: { $eq: ["$$val", "exchange bike"] }, then: "Exchange Bike" },
                                            { case: { $eq: ["$$val", "finance pending"] }, then: "Finance Pending" },
                                            { case: { $eq: ["$$val", "follow up"] }, then: "Follow Up" },
                                            { case: { $eq: ["$$val", "helmet free"] }, then: "Helmet Free" },
                                            { case: { $eq: ["$$val", "manager discount"] }, then: "Manager Discount" },
                                            { case: { $eq: ["$$val", "ok"] }, then: "OK" },
                                            { case: { $eq: ["$$val", "old bike exchange"] }, then: "Old Bike Exchange" },
                                            { case: { $eq: ["$$val", "price mismatch"] }, then: "Price Mismatch" },
                                            { case: { $eq: ["$$val", "same as above"] }, then: "Same As Above" },
                                            { case: { $eq: ["$$val", "see notes"] }, then: "See Notes" },
                                            { case: { $eq: ["$$val", "test entry"] }, then: "Test Entry" },
                                            { case: { $eq: ["$$val", "waiting for RTO"] }, then: "Waiting for RTO" }
                                        ],
                                        default: "$$val"
                                    }
                                }
                            ]
                        }
                    }
                }
            }
        }
    ]
);
```

## 2. Clean the `delivery` column

```javascript
db.cleaning.updateMany(
  {},
  [
    {
      $set: {
        delivery: {
          $switch: {
            branches: [
              {
                case: {
                  $in: [
                    { $toLower: { $trim: { input: { $toString: { $ifNull: ["$delivery", ""] } } } } },
                    ["yes", "y", "1", "true", "done"]
                  ]
                },
                then: "Yes"
              },
              {
                case: {
                  $in: [
                    { $toLower: { $trim: { input: { $toString: { $ifNull: ["$delivery", ""] } } } } },
                    ["no", "n", "0", "false"]
                  ]
                },
                then: "No"
              }
            ],
            default: "Unknown"
          }
        }
      }
    }
  ]
);
```

## 3. Clean the `test_ride` column

```javascript
db.cleaning.updateMany(
  {},
  [
    {
      $set: {
        test_ride: {
          $switch: {
            branches: [
              {
                case: {
                  $in: [
                    {
                      $toLower: {
                        $trim: {
                          input: {
                            $toString: {
                              $ifNull: ["$test_ride", ""]
                            }
                          }
                        }
                      }
                    },
                    ["yes", "y", "1", "true", "done"]
                  ]
                },
                then: "Yes"
              },
              {
                case: {
                  $in: [
                    {
                      $toLower: {
                        $trim: {
                          input: {
                            $toString: {
                              $ifNull: ["$test_ride", ""]
                            }
                          }
                        }
                      }
                    },
                    ["no", "n", "0", "false"]
                  ]
                },
                then: "No"
              }
            ],
            default: "Unknown"
          }
        }
      }
    }
  ]
);
```

## 4. Clean the `Gender` column

```javascript
db.cleaning.updateMany(
  {},
  [
    {
      $set: {
        Gender: {
          $switch: {
            branches: [
              {
                case: {
                  $in: [
                    {
                      $toLower: {
                        $trim: {
                          input: {
                            $convert: {
                              input: "$Gender",
                              to: "string",
                              onError: "",
                              onNull: ""
                            }
                          }
                        }
                      }
                    },
                    ["m", "male", "man"]
                  ]
                },
                then: "Male"
              },
              {
                case: {
                  $in: [
                    {
                      $toLower: {
                        $trim: {
                          input: {
                            $convert: {
                              input: "$Gender",
                              to: "string",
                              onError: "",
                              onNull: ""
                            }
                          }
                        }
                      }
                    },
                    ["f", "female", "woman"]
                  ]
                },
                then: "Female"
              },
              {
                case: {
                  $in: [
                    {
                      $toLower: {
                        $trim: {
                          input: {
                            $convert: {
                              input: "$Gender",
                              to: "string",
                              onError: "",
                              onNull: ""
                            }
                          }
                        }
                      }
                    },
                    ["o", "other", "nb", "x"]
                  ]
                },
                then: "Other"
              }
            ],
            default: "Unknown"
          }
        }
      }
    }
  ]
);
```

## 5. Clean the `customer_age` column

```javascript
db.cleaning.updateMany(
  {},
  [
    {
      $set: {
        customer_age: {
          $let: {
            vars: {
              age: {
                $toLower: {
                  $trim: {
                    input: {
                      $convert: {
                        input: "$customer_age",
                        to: "string",
                        onError: "",
                        onNull: ""
                      }
                    }
                  }
                }
              }
            },
            in: {
              $let: {
                vars: {
                  cleaned: {
                    $trim: {
                      input: {
                        $replaceAll: {
                          input: {
                            $replaceAll: {
                              input: "$$age",
                              find: "years",
                              replacement: ""
                            }
                          },
                          find: "yrs",
                          replacement: ""
                        }
                      }
                    }
                  }
                },
                in: {
                  $cond: [
                    {
                      $regexMatch: {
                        input: "$$cleaned",
                        regex: /^(1[5-9]|[2-6][0-9]|7[0-8])$/
                      }
                    },
                    { $toInt: "$$cleaned" },
                    "Unknown"
                  ]
                }
              }
            }
          }
        }
      }
    }
  ]
);
```

## 6. Count duplicate inputs

```javascript
db.cleaning.aggregate([
    {
        $group: {
            _id: {
                invoice_id: "$invoice_id",
                customer_id: "$customer_id"
            },
            count: { $sum: 1 }
        }
    },
    {
        $match: {
            count: { $gt: 1 }
        }
    },
    {
        $count: "total_duplicate_groups"
    }
])
```

## 7. Delete duplicate values

```javascript
db.cleaning.aggregate([
    {
        $sort: { _id: 1 }
    },
    {
        $group: {
            _id: {
                invoice_id: "$invoice_id",
                customer_id: "$customer_id"
            },
            ids: { $push: "$_id" }
        }
    },
    {
        $match: {
            $expr: { $gt: [{ $size: "$ids" }, 1] }
        }
    }
]).forEach(doc => {
    db.cleaning.deleteMany({
        _id: { $in: doc.ids.slice(1) }
    });
});
```
