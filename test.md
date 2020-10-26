from torch.utils.data import DataLoader
from torch.utils.data import Dataset
from torch import nn
from torch import optim
import glob
import os
import numpy as np
from PIL import Image
from torch.autograd import Variable
import torch
import torch.nn.functional as F

## load mnist dataset
use_cuda = torch.cuda.is_available()

trainPath = "D:\\practice\\pytorch\\2.5_t_classifier\\dataset\\mnistasjpg\\trainingSet\\trainingSet\\*\\*.jpg"
trainTestPath = "D:\\practice\\pytorch\\2.5_t_classifier\\dataset\\mnistasjpg\\trainingSample\\trainingSample\\*\\*.jpg"


# 定义数据源
class MnistDataset(Dataset):
    def __init__(self, root_dir):
        self.files = glob.glob(root_dir)
        self.x = 123

    def __len__(self):
        return len(self.files)

    def __getitem__(self, idx):
        img = np.asarray(Image.open(self.files[idx])).reshape((-1, 28, 28))
        label = os.path.abspath(self.files[idx]).split("\\")[-2]
        return img, label


trainMiniSet = MnistDataset(trainPath)
trainTestMiniSet = MnistDataset(trainTestPath)
trainDataLoader = DataLoader(trainMiniSet, batch_size=100, num_workers=0, shuffle=True)
trainTestDataLoader = DataLoader(trainTestMiniSet, batch_size=100, num_workers=0, shuffle=True)


print('==>>> total trainning batch number: {}'.format(len(trainDataLoader)))
print('==>>> total testing batch number: {}'.format(len(trainTestDataLoader)))


## network
class MLPNet(nn.Module):
    def __init__(self):
        super(MLPNet, self).__init__()
        self.fc1 = nn.Linear(28 * 28, 500)
        self.fc2 = nn.Linear(500, 256)
        self.fc3 = nn.Linear(256, 10)

    def forward(self, x):
        x = x.view(-1, 28 * 28)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

    def name(self):
        return "MLP"


class LeNet(nn.Module):
    def __init__(self):
        super(LeNet, self).__init__()
        self.conv1 = nn.Conv2d(1, 20, 5, 1)
        self.conv2 = nn.Conv2d(20, 50, 5, 1)
        self.fc1 = nn.Linear(4 * 4 * 50, 500)
        self.fc2 = nn.Linear(500, 10)

    def forward(self, x):
        x = F.relu(self.conv1(x.float()))
        x = F.max_pool2d(x, 2, 2)
        x = F.relu(self.conv2(x))
        x = F.max_pool2d(x, 2, 2)
        x = x.view(-1, 4 * 4 * 50)
        x = F.relu(self.fc1(x))
        x = self.fc2(x)
        return x

    def name(self):
        return "LeNet"


## training
model = LeNet()

if use_cuda:
    model = model.cuda()

optimizer = optim.SGD(model.parameters(), lr=0.02, momentum=0.9)

criterion = nn.CrossEntropyLoss()

for epoch in range(5):
    # trainning
    ave_loss = 0
    for batch_idx, (x, target) in enumerate(trainDataLoader):
        optimizer.zero_grad()
        if use_cuda:
            x, target = x.cuda(), target.cuda()
        label = np.array(target, dtype=float)
        label_tendor = torch.from_numpy(label)
        x, target = Variable(x), Variable(label_tendor)
        x = x.float()
        out = model(x)
        loss = criterion(out, target.long())
        ave_loss = ave_loss * 0.9 + loss.item() * 0.1
        loss.backward()
        optimizer.step()
        if (batch_idx + 1) % 100 == 0 or (batch_idx + 1) == len(trainDataLoader):
            # print('==>>> epoch: {}, batch index: {}, train loss: {:.6f}'.format(  epoch, batch_idx + 1, ave_loss))
            print('==>>> epoch: {}, batch index: {}, train loss: {:.6f}'.format(  epoch, batch_idx + 1, loss.item()))
    # testing
    correct_cnt, ave_loss = 0, 0
    total_cnt = 0
    for batch_idx, (x, target) in enumerate(trainTestDataLoader):
        if use_cuda:
            x, target = x.cuda(), target.cuda()
        label = np.array(target, dtype=float)
        label_tendor = torch.from_numpy(label)
        with torch.no_grad():
            x, target = Variable(x), Variable(label_tendor)
        out = model(x)
        loss = criterion(out, target.long())
        _, pred_label = torch.max(out.data, 1)
        total_cnt += x.data.size()[0]
        correct_cnt += (pred_label == target.data).sum()
        # smooth average
        ave_loss = ave_loss * 0.9 + loss.item() * 0.1

        if (batch_idx + 1) % 100 == 0 or (batch_idx + 1) == len(trainTestDataLoader):
            print('==>>> epoch: {}, batch index: {}, test loss: {:.6f}, acc: {:.3f}'.format(epoch, batch_idx + 1, ave_loss, correct_cnt * 1.0 / total_cnt))

torch.save(model.state_dict(), model.name())
