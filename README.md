import java.util.*;

public class Network {
    private List<List<Link>>[] connections;

    public Network(int layers, int[] nodesInLayer) {
        connections = new ArrayList[layers - 1];
        for (int i = 0; i < layers - 1; i++) {
            connections[i] = new ArrayList<>();
            for (int j = 0; j < nodesInLayer[i]; j++) {
                List<Link> nodeLinks = new ArrayList<>();
                for (int k = 0; k < nodesInLayer[i + 1]; k++) {
                    nodeLinks.add(new Link(0.0));
                }
                connections[i].add(nodeLinks);
            }
        }
    }

    public void setLinkWeight(int layer, int fromNode, int toNode, double weight) {
        connections[layer - 1].get(fromNode).get(toNode).setValue(weight);
    }

    public double getLinkWeight(int layer, int fromNode, int toNode) {
        return connections[layer - 1].get(fromNode).get(toNode).getValue();
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the number of layers: ");
        int layers = scanner.nextInt();
        int[] nodesInLayer = new int[layers];
        for (int i = 0; i < layers; i++) {
            System.out.print("Enter the number of nodes in layer " + i + ": ");
            nodesInLayer[i] = scanner.nextInt();
        }
        Network network = new Network(layers, nodesInLayer);
        System.out.println("Enter the link weights:");
        for (int i = 0; i < layers - 1; i++) {
            for (int j = 0; j < nodesInLayer[i]; j++) {
                for (int k = 0; k < nodesInLayer[i + 1]; k++) {
                    System.out.print("Enter the weight for link from node " + j + " in layer " + (i + 1) + " to node " + k + " in layer " + (i + 2) + ": ");
                    double weight = scanner.nextDouble();
                    network.setLinkWeight(i + 1, j, k, weight);
                }
            }
        }
        System.out.println("Enter the node indices to query the weight (layer, fromNode, toNode):");
        int layer = scanner.nextInt();
        int fromNode = scanner.nextInt();
        int toNode = scanner.nextInt();
        double weight = network.getLinkWeight(layer, fromNode, toNode);
        System.out.println("Weight: " + weight);
    }
}

class Link {
    private double value;

    public Link(double value) {
        this.value = value;
    }

    public void setValue(double value) {
        this.value = value;
    }

    public double getValue() {
        return value;
    }
}
