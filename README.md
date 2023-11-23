# rest-API
const express=require("express");
const app=express();
 const mysql=require("mysql");
 const bodyParser= require("body-parser");
 const port=8000;

            //Middleware

            app.use(bodyParser.json());
            app.use(bodyParser.urlencoded({
                extended:true
            }));

            //Database 

       const con= mysql.createConnection({
 host:"localhost",
   user:"root",
 password:"",
  database:"data"
 })

            // Connection

       con.connect(function(error){
          if(error){
        console.log("Not Connected");
             }else{
     console.log("Connected");
                }
            });

            app.get("/api/delivery",function(req,res){
                let query="SELECT * FROM delivery";
                con.query(query,function(error,results){
                    if(error){
                       res.send({status:false,message:"Failed to read data"})
                    }else{
                        res.send({status:true,data:results})
                    }
                });
            });
            //Show One data

            app.get("/api/delivery/show/:id",function(req,res){
                let query="SELECT * FROM delivery WHERE ID=?";
                let id=[req.params.id];
                con.query(query,id,(error,field)=>{
                    if(error){
                        res.send({status:false,message:"Failed To Show"});
                    }else{
                        res.send({status:true,data:field})
                    }
                });
            });


            // INSERT DATA

            app.post("/api/delivery/ins",(req,res)=>{
                let pquery="INSERT INTO delivery ( NAME , RATE, QUANTITY ) VALUES(?,?,?)";
              let ins =[req.body.NAME , req.body.RATE , req.body.QUANTITY];  
                con.query(pquery,ins,function(error,results){
                   
                    if(error){
                        console.error(error);
                        res.send({status:false,message:"Failed to insert"});
                    }else{
                        res.send({status:true,data:results})
                    }
                });
            });


            //update

            app.put("/api/delivery/up/:id",(req,res)=>{
                let pquery="UPDATE  delivery  SET NAME = ? , RATE = ?, QUANTITY = ?  WHERE ID = ?";
              let update =[req.body.NAME , req.body.RATE , req.body.QUANTITY , req.params.id];
                con.query(pquery,update,(error,results)=>{
                    if(error){
                        console.error(error);
                        res.send({status:false,message:"Failed to insert"});
                    }else{
                        res.send({status:true,data:results})
                    }
                });
            });
                //Delete data
  app.delete("/api/delivery/del/:id",function(req,res){
    let query="DELETE  FROM  delivery WHERE ID= ?";
    let del=[req.params.id];
    con.query(query,del,(error,results)=>{
        if (error){
            res.send({status:false,message:"failed to delete"});
        }else{
            res.send({status:true,message:"Successfully delete "});
        }
    });
  });




    // listen PORT 

    app.listen(port,function(){
        console.log(port+" is running");
    });
