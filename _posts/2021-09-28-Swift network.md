---
title: Swift Network
date: 2021-09-28
layout: post
tags: Swift, iOS
---


# Swift Network

1. Create struct comply with Decodable protocol

The parameter name should same as json key name

```swift
struct Agent: Decodable {
    let id: Int
    let name: String
    let regno: String
    let area: String
    let description: String
    let achievement: [String]
}

struct Agents: Decodable {
    let agents: [Agent]
}
```

2. Request data and decode as struct 

```swift
AF.request(serverurl + "/randomagent").responseDecodable (of: Agents.self) { (response) in
    guard let agents = response.value else {return}
    self.current_agents = agents
    NotificationCenter.default.post(name: agentNotification, object: nil, userInfo: nil)
}
// Request with closure

AF.request(serverurl + "/randomareaagent",
       parameters: ["area": area],
       encoding: URLEncoding.queryString).responseDecodable(of: Agents.self) { (response) in
    guard let records = response.value else {return}
    completion(records.agents)
}
```

3. Server side (Flask)

```python

def cvdict_agent(onerecord, curr):
    d = {}
    d['id'] = int(onerecord[0])
    d['name'] = onerecord[2]
    d['regno'] = onerecord[1]
    if onerecord[4]:
        d['area'] = onerecord[4]
    else:
        d['area'] = ''
    d['description'] = onerecord[3]
    sql1 = '''select achievement from achievement where
    regno="{}" order by id desc limit 10'''.format(d['regno'])
    d['achievement'] = [x[0] for x in curr.execute(sql1).fetchall()]
    return d

@app.route('/randomagent')
def randomAgent():
    db = sqlite3.connect('data.db')
    curr = db.cursor()
    sql = '''select * from agent order by random() limit 20'''
    r = curr.execute(sql).fetchall()
    r = [cvdict_agent(x, curr) for x in r]
    return jsonify({'agents': r})

```

