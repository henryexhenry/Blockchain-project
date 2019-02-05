# Symple Blockchain System
- Brief introduction
In this project, a basic prototype of blockchain has been implemented. 
This is a simplified Bitcoin system which include some essential components of bitcoin, 
such as decentralized networking, proof of work algorithm, transaction pool, merkle root, etc.
The system is written by Python, and MongoDB is involved in our system.

    -Block prototype
        1. index
        2. hash
        Return by gen_hash function
        3. pv_hash
        The hash value of the previous block
        4. Nonce
        A counter used for POW algorithm
        5. tx_data
        A string getting from user
        6. merkle root
        a hash value for maintaining the originality of the transaction data.
        return by merkle_root function
            - implementation:
            get two transactions from transaction pool
            check whether any input value is empty

            if there are two non-empty transaction,
            we add the two string together and hash them with `hashlib.sha256(root.encode()).hexdigest()`

            if there is only one non-empty transaction, we copy it and add the two same txs together
            we should first encode the sum string into byte format, 
            then apply hash function 
        7. spendtime
        Time that we need for mining a block
    - Decentralized network
    After each node successfully mined a new block, it will send message to neighbor nodes.
    Now let say Node 1 has successfully mined a new block, it has to send the block data to node 2. In this situation, Node 2 acts as a server node and Node 1 will be the client node.
    Node 2 will first create a server socket at port 8002 by following python code
    `
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.bind((HOST, my_PORT))
    `
    then it wait for incoming connection request by code `s.listen()`.

    Client node also create a client socket with the first line of above code.
    Then Node 1 initiates connection to port 8002 by code
    `s.connect((HOST, 8002))`
    After three-way-handshake protocol, a TCP connection will be established.
    
    Now Node 1 can send data to node 2. Before sending data, the data type should be
    encoded into bytes by using utf-8 encoding. Then the data will be sent by the code 
    `s.sendall(data)`. The data Node 1 sent is hash value of the new mined block.

    By using the code `data = conn.recv(1024).decode("utf-8")`, the data is converted 
    back to string, and read by node 2.

    Now the data transfering between 2 nodes is completed. Two nodes will now close the connection
    by using code `s.close()`