# smartCustomer

可用于批量创建全局或局部自定义数量并发消费者，并使用自定义方法消费  

使用：  
自定义消费方法PrintData，在初始化smartCustomer时，传入即可，具体查看main方法



    func main() {
	    smartCustomer := core.NewSmartCustomer(3,PrintData)

	    i := 0
        for {
            i ++
            smartCustomer.AddDataQueue(i)
            time.Sleep(time.Second*1)
        }
        
        
        //Clever
        cleverCustomer := core.NewCleverCustomer(10, 0, PrintData)
    
        var err error
        err = cleverCustomer.NewClever("no_1", 1,nil)
        if err != nil {
            log.Println(err)
        }
        err = cleverCustomer.NewClever("no_2", 2,PrintData2)
        if err != nil {
            log.Println(err)
        }
    
        fmt.Println("size: ", cleverCustomer.GetCleverSize())
    
        go func() {
            i := 0
            for {
                i ++
                cleverCustomer.AddSmartData("no_1", "这是no_1---"+cast.ToString(i))
                time.Sleep(time.Second * 1)
                if i == 5 {
                    time.Sleep(time.Second * 12)
                }
            }
        }()
        go func() {
            i := 0
            for {
                i ++
                cleverCustomer.AddSmartData("no_2", "这是no_2---"+cast.ToString(i))
                time.Sleep(time.Second * 1)
                if i == 5 {
                    time.Sleep(time.Second * 12)
                }
            }
        }()
        select {}

    }

    func PrintData(data interface{})  {
    	fmt.Println(data)
    	time.Sleep(time.Second*3)
    }
